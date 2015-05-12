---
title: HikariBot IRC Bot
date: 2015-05-10
css:
- /assets/css/headerbox.css
---

[abthik]: /about/boxen.html#hikari
[brst]: /resume/#brst
[brsttwit]: https://twitter.com/Bronystate
[pb]: http://www.jibble.org/pircbot.php
[pc]: http://www.ponychat.net
[pcircd]: https://github.com/Elemental-IRCd/elemental-ircd "IRC daemon, in this case Elemental-IRCd"

# HikariBot

![HikariBot icon](bigicon-hikaribot.png){: .noborder}
{: .floatright}

- Source: <http://bitbucket.org/shizuka/hikaribot>
- [PircBot-based][pb] IRC bot
- Operates as **Roseluck** on [PonyChat][pc]
- Currently **OFFLINE** due to [hikari][abthik] issues
{: .headerbox}

### Components
- [Commands](#commands)
- [Permissions](#permissions)
- [Twitter Integration](#twitbot)
- [Banhammer](#banhammer)

---------------------------------------------------------------------------

## Overview {#overview}

HikariBot is a PircBot IRC bot built for [Bronystate][brst], with two major
goals. First, to provide an in-chat command to post a tweet to the
[@Bronystate][brsttwit] Twitter account, and second, to manage the rather
high number of user bans in the channel. In addition, the command handling
needed to be easy to add new commands to, and of course we needed to ensure
only Bronystate staff could actually command it.

The project was my first real attempt to extend an existing API with Java,
and to essentially create my own on top of it. Though some parts of its
functionality are dependent on [PonyChat's IRCd][pcircd], and for the
foreseeable future it will only be deployed there, I tried to make it as
platform-agnostic as I could. There are parts of it that could be designed
much better, I'm sure, but overall it works.

---------------------------------------------------------------------------

## Commands {#commands}

``` java
//abbreviated from sk.hikaribot.twitter.cmd.Tweet
public class Tweet extends Command {
  public Tweet() {
    this.name = "tweet";
    this.numArgs = 1;
    this.helpArgs.add("message");
    this.helpInfo = "tweet to the current profile";
    this.reqPerm = 2; //ops
  }
  
  @Override
  public void execute(String channel, String sender, String params) {
    TwitBot twit = bot.getTwitBot();
    //log: TWEET FROM sender IN channel: params
    try {
      Status tweet = twit.tweet(params);
      //tweet echo now handled by TwitListener
      //log: TWEET OK: url of the tweet posted
    } catch (NoProfileLoadedException ex) {
      //log that no AccessToken is loaded
    } catch (TweetTooLongException ex) {
      //log that the tweet was too long, doesn't shorten URLs
    } catch (TwitterException ex) {
      //log that something went wrong, print the ex.Message()
    }
  }
  
  @Override
  public void execute(String channel, String sender) throws ImproperArgsException {
    throw new ImproperArgsException(this.name); //invokes '.help tweet'
  }
}
```

HikariBot needs commands in order to be useful to the channel, and rigidly
coding them all into its message-handling is a Bad Thing. Right away, I set
up a `CommandRegistry`, a list of all available commands, and the classes
that contain their actual logic. All messages that start with a certain
character --- in Roseluck's case, a period --- get passed along to the
registry, which checks its list to see if the command exists. If it does,
it then calls the `PermissionsManager` to make sure the message's sender is
allowed to run the command, and finally calls into the command object to do
work.

Commands are initialized with a reference to `HikariBot`, which itself has
methods to point at its components. A command that needs to use Twitter API
functions can fetch the `TwitBot` component from `HikariBot`, and make the
call itself. Commands all inherit a common logger object, and through the
reference to `HikariBot` can send messages to IRC, targeted at the channel
and sender of the original command.

At first, I wanted to use autoloading to populate the `CommandRegistry`'s
list, but couldn't wrap my head around how to do it. It seemed simple
enough to me --- for all classes that extend `Command` in this package,
make an object out of them and pass it to registry --- but in practice the
flow of code didn't make enough sense. So, commands are added to the list
one by one during `HikariBot`'s startup:

``` java
//...
cr.add(new sk.hikaribot.cmd.Join());
cr.add(new sk.hikaribot.cmd.Part());
cr.add(new sk.hikaribot.cmd.Say());
cr.add(new sk.hikaribot.cmd.Nick());
cr.add(new sk.hikaribot.cmd.DoAction());
cr.add(new sk.hikaribot.cmd.Version());
//...
```

There's also the issue of interacting with IRC being an asynchronous
process. The bot can send messages, but doesn't typically expect a reply.
And any reply it does get tends to be another command. Talking to the IRC
server directly is a different matter, such as getting a WHOIS on a user.
The bot sends the WHOIS request, but the server will send several lines
back, or possibly none at all, and none of the lines will pass through the
bot's normal command handler.

So, commands that need to wait for a server response, or any component that
sends requests to the server, creates and subscribes and object to
`ServerResponse`, which intercepts server lines and passes them along. The
command, or component, then subscribes itself to this handler, which is
responsible for decoding the line and deciding when it's done gathering
input. The handler then notifies its own observers, which is the command
that created it, which then takes the gathered input, unsubscribes the
handler from `ServerResponse`, and destroys the handler, going on with its
own logic with the input.

``` java
//abbreviated from sk.hikaribot.banhammer.api.BanChannel
private void requestBanlist() {
  BanlistResponse br = new BanlistResponse(channel);
  br.addObserver(this);
  bot.getServerResponder().addObserver(br);
  //log: channel REQUESTING BANLIST...
  bot.sendRawLine("MODE " + channel + " +b");
}

@Override
public void update(Observable o, Object arg) {
  if (o instanceof BanlistResponse) {
    //don't know why it ever wouldn't be but sanity check nonetheless
    List<ScrapedBan> sb = (List<ScrapedBan>) arg;
    bot.getServerResponder().deleteObserver((Observer) o);
    this.scrapeBanlist(sb);
  }
}

private void scrapeBanlist(List<ScrapedBan> scrapedBans) {
  //log: channel GOT BANLIST...
  //proceed to do logic against a list of banmasks, setters, and timestamps
}
```
``` java
//abbreviated from sk.hikaribot.banhammer.api.BanlistResponse
@Override
public void update(Observable o, Object arg) {
  String line = arg.toString();
  String[] serverResponse = line.split(":", 2);
  int code = Integer.parseInt(serverResponse[0]);
  String response = serverResponse[1];
  if (!response.split(" ")[1].equals(channel)) {
    //we don't care about messages for channels other than ours
    return;
  }
  if (code == RPL_BANLIST) { //367
    this.onBanlist(response); //parses the ban information from the line
  } else if (code == RPL_ENDOFBANLIST) { //368
    this.onEndOfBanlist(); //this.setChanged() then this.notifyObservers()
  }
}
```

It's a mess, and feels like a flagrant abuse of the Observer/Observable
pattern, but it works, and works alarmingly well. Commands can easily wire
up a response handler, such as for WHOIS or a list of bans, and be given
the input whenever the server sends it, which could be immediately or after
a long period of time. This prevents a server request from blocking the
bot's handling of new messages, and keeps it all responsive.

---------------------------------------------------------------------------

## Permissions {#permissions}

``` java
/**
 * Gets userlevel for a nick.
 *
 * @param nick the nick to fetch userlevel for, checks current nick first,
 * then nick as canonical name
 * @return account's userlevel, 0 if account doesn't exist or isn't identified
 */
public int getUserLevel(String nick) {
  if (isIdentified(nick)) {
    return accounts.get(cache.get(nick));
  } else {
    return 0; //no special treatment for an unidentified account
  }
}
```

The Permissions component ensures that only authorized nicks can
successfully invoke a command. It relies on Nickserv services to better
identify a nick as an individual person it can trust, so that the person
could change nicks and HikariBot still recognizes them. When it joins a
channel, it queries the channel to receive the Nickserv accounts, if any,
that all the users in the channel are identified for. It then compares to
an offline config file for its actual list of authorized accounts, and if
an account matches, it stores the current nick, the canonical Nickserv
nick, and the permissions level in a pair of HashMaps (one for nick->nick,
the other for nick->level).

Identifying users automatically when *they* join was a different matter,
requiring the bot report extended capabilities to the IRCd, which responds
by sending extra information on joins. For HikariBot, the server will send
an extra `ACCOUNT` line along with `JOIN` lines, indicating the Nickserv
account the user is identified for. Before implementing the extended-join,
HikariBot relied on a `.identify` command, performing a WHOIS against the
sender to pick up the Nickserv account.

When a user leaves a channel or quits from a channel, where HikariBot can
see it, it can no longer be certain that nick still belongs to who it
thinks it does --- someone else could join with the nick, and after leaving
the channel the user could quit from somewhere else. The bot passes along
`onPart()` and `onQuit()` events it sees in channels, and `Permissions`
reacts by deauthorizing the nick that just left, if it was authorized to
begin with. This generally worked so long as the bot was only sitting in
Bronystate's main channel, but wouldn't scale to sitting in other channels
on the same network. Leaving any channel would trigger the bot to forget
the nick, even if it was still in another channel with it.

``` java
public void onPart(String nick) {
  if (isIdentified(nick)) {
    for (String channel : bot.getChannels()) {
      if (bot.getUser(channel, nick) != null) {
        //user is in one of our remaining channels
        //log: USER foo PARTED, BUT FOUND IN #channel
        return;
      }
    }
    //user has PARTED completely out of sight
    //log: USER foo PARTED, NO LONGER VISIBLE, LOGGING OUT
    cache.remove(nick);
  }
}
```

The solution was to have the bot scan all its channels to try and find a
nick that just left a channel it could see. If the nick was still visible
in some other channel, the bot makes a note of the `/part`, and carries on.
If the user isn't visible, the bot can't trust the next time it sees that
nick, so it removes it from the cache. The canonical nick still has a
permissions level, but the bot won't trust them as nicks until it verifies
with Nickserv that the nick is used by who it thinks should.

While users parting wasn't a major issue --- generally people quit the
server entirely rather than just leave the channel --- this setup in logic
made the bot less error-prone, and it performs sensibly. Unfortunately,
this introduced a new bug: what happens if the *bot* is the one leaving a
channel?

``` java
public void onBotPart(String channelParted) {
  //log: PARTING CHANNEL #channel...
  //we parted a channel, we need to check all idented users
  String[] channels = bot.getChannels();
  Set<String> nicks = cache.keySet();
  List<String> nicksToRemove = new ArrayList<>();
  userLoop:
  for (String nick : nicks) {
    for (String channel : channels) {
      if (channel.equals(channelParted)) {
        continue; //we just left here! can't check what we can't see!
      }
      User[] users = bot.getUsers(channel);
      for (User user : users) {
        if (user.equals(nick)) {
          //log: USER foo STILL VISIBLE
          continue userLoop; //found it, don't need to check the user anymore
        }
      }
    }
    //log: USER foo LOST, LOGGING OUT
    nicksToRemove.add(nick);
  }
  for (String nick : nicksToRemove) {
    cache.remove(nick);
  }
}
```

My first Java `GOTO`. As the bot leaves a channel, it must verify that the
list of nicks it has authorized are still visible somewhere in the channels
it's still in. I don't like the triple `for` loop nest, but couldn't think
of a better way than brute force searching all nicks in all channels to
ensure every nick in its cache was present. I'm not that ashamed of the
`continue userLoop;` line, since escaping the nested `for` loops was
otherwise only possible by having a boolean track the search effort through
them, then riding `break`s back out. Extra lines, extra thinking per loop,
when good old `GOTO` is a good solution to the issue.

It's still a `GOTO`, it's still considered harmful.

---------------------------------------------------------------------------

## Twitter Integration {#twitbot}

``` java
public String loadAccessToken(String profile) throws IOException, TokenMismatchException, 
                                                     MissingRequiredPropertyException,
                                                     TwitterException {
  profile = profile.toLowerCase();
  //log: Loading token for profile...
  FileReader proFile = new FileReader("@" + profile + ".properties");
  Properties props = new Properties();
  props.load(proFile);
  if (!props.getProperty("accessToken.name").equals(profile)) {
  //no reason we should fetch @bar if we wanted @foo.properties
    throw new TokenMismatchException(profile, props.getProperty("accessToken.name"));
  }
  this.sanityCheckToken(props); //throws MissingRequiredPropertyException
  //log: Token for profile is sane.
  String token = props.getProperty("accessToken.token");
  String tokenSecret = props.getProperty("accessToken.secret");
  this.clearAccessToken();
  this.setAccessToken(new AccessToken(token, tokenSecret));
  return twit.verifyCredentials().getScreenName();
}
```

HikariBot's purpose from the outset was to help Bronystate's staff make
better use of our official Twitter account. We all had the necessary
credentials, but only a handful of us actually use Twitter on a regular
basis, and the rest couldn't be bothered swapping between their normal
account and the Bronystate account for one-off announcements. In early
2012, we had grand visions of automatically announcing streams, scheduled
tweets, and all manner of interaction with our viewers, followers, and
fans, but it took until late 2014 for me to finally start on the project.

Ultimately, this component provides one command, `.tweet`, that does
exactly that: sends a tweet. To get there required learning a good deal of
the Twitter API, finding a Java library that could interface with it, and
implementing a good deal of logic just to handle the bot authenticating
itself to post tweets to an account. This ballooned into several other
commands to manage the bot's relationship with Twitter, without needing to
start the bot all over.

On startup, `Main` sanity checks a config file containing the Twitter API
keys for HikariBot, then as `HikariBot` is initializing, it starts the
`TwitBot` component, which sets up the objects and state necessary to talk
to Twitter. It doesn't do anything yet, but is ready to. Then the
`.twitload <profile>` command is run from IRC. The bot looks for a file
containing that profile's authentication token, if it has one, and loads
that into its state. After that point, `.tweet <message>` will cause it to
post a tweet, using the profile's token. The result: the profile tweets,
and closer inspection of the raw tweet shows it came from HikariBot.

That's the easy part. I also created a chain of commands, `.twitrequest`
and `.twitconfirm` to handle the creation of profile tokens automatically.
The `.twitrequest` command creates a URL and privately messages it to the
sender, who then visits it while logged in as the target profile. They're
given a unique key, which they pass to HikariBot through `.twitconfirm`,
which transforms a temporary request token into a permanent token, which is
then stored to file.

Making the bot echo the tweet back out was more difficult. Sure, the API
returns a URL to the tweet, and from that the bot can request the tweet's
contents --- once a command finishes, HikariBot forgets everything about
it. But some streaming clients could automatically tweet when they started
streaming, provided a profile to tweet to, and we wanted the bot to be able
to echo these tweets, even though it didn't send them itself. This required
learning the TwitterStream API, and wading through objects necessary to
tell Twitter "hey can you send me any tweets this user makes?", and filters
to ensure what we received was what we wanted going to channel.

After hours of coding, and several more debugging edge cases, the listener
worked, adding `.twitassign <channel>`, `.twitfollow`, and `.twitlisten` to
the list of Twitter-related commands, to select a channel, a user, and to
start echoing respectively. This could still fail in random ways, now that
the bot was making a separate connection over the internet unrelated to the
relatively-stable IRC pipe, and there is (as yet) no recovery if the link
fails and the library's reconnection attempts fail. Still, the failure is
well documented in the bot's console logs, making it easy to see, if late,
when the component has failed.

---------------------------------------------------------------------------

## Banhammer {#banhammer}

### Development on hold due to [Hikari][abthik] issues

What did I get myself into?

``` java
/*
 * EVIL SIDE EFFECT CODE BEGINS
 */
stat.execute("CREATE TABLE IF NOT EXISTS 'bh_" + channel + "_bans'(banId INTEGER PRIMARY KEY AUTOINCREMENT, type, banMask UNIQUE, author, timeSet)");
stat.execute("CREATE TABLE IF NOT EXISTS 'bh_" + channel + "_notes'(noteId INTEGER PRIMARY KEY AUTOINCREMENT, banId, timestamp, author, type, note)");

//When a new ban is scraped (type S)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_scrapedNewBan' AFTER INSERT ON 'bh_" + channel + "_bans' FOR EACH ROW WHEN NEW.type='S' BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','A','Newly found on scraped list, set by '||NEW.author); "
        + "UPDATE 'bh_" + channel + "_bans' SET type='A' WHERE banId=NEW.banId; "
        + "END;");

//When a ban is set by command (that didn't exist) (type N)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_newBan' AFTER INSERT ON 'bh_" + channel + "_bans' WHEN NEW.type='N' BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,strftime('%s','now'),'Banhammer','A','Ban set by '||NEW.author); "
        + "UPDATE 'bh_" + channel + "_bans' SET type='A' WHERE banId=NEW.banId; "
        + "END;");

//When an existing ban is scraped (type S where OLD.type not A/P/T)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_scrapedOldBan' AFTER UPDATE OF type ON 'bh_" + channel + "_bans' FOR EACH ROW WHEN NEW.type='S' AND OLD.type NOT IN ('A') BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','A','Found on scraped list, set by '||NEW.author); "
        + "UPDATE 'bh_" + channel + "_bans' SET type='A' WHERE banId=NEW.banId; "
        + "END;");

//When a ban is set active (type A/N where OLD.type not A/N/S)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_activateBan' AFTER UPDATE OF type ON 'bh_" + channel + "_bans' WHEN NEW.type IN ('A', 'N') AND OLD.type NOT IN ('A', 'N', 'S') BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','A','Ban re-set by '||NEW.author); "
        + "UPDATE OR IGNORE 'bh_" + channel + "_bans' SET type='A' WHERE banId=NEW.banId; "
        + "END;");

//When a ban is set inactive (type I)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_inactivateBan' AFTER UPDATE OF type ON 'bh_" + channel + "_bans' WHEN NEW.type='I' BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','I','Rotated out of list'); "
        + "END;");

//When a ban is unset due to missing from scrape (type M set when OLD.type not I/U)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_missingBan' AFTER UPDATE OF type ON 'bh_" + channel + "_bans' FOR EACH ROW WHEN NEW.type='M' BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','U','Missing from scraped list'); "
        + "UPDATE 'bh_" + channel + "_bans' SET type='U' WHERE banId=NEW.banId; "
        + "END;");

//When ban is unset by command (type U where OLD.type not M)
stat.execute("CREATE TRIGGER IF NOT EXISTS 'bh_" + channel + "_unsetBan' AFTER UPDATE OF type ON 'bh_" + channel + "_bans' WHEN NEW.type='U' AND OLD.type!='M' BEGIN "
        + "INSERT INTO 'bh_" + channel + "_notes'(banId,timestamp,author,type,note) VALUES (NEW.banId,NEW.timeSet,'Banhammer','U','Ban unset by '||NEW.author); "
        + "END;");

//The most recently set Active ban
stat.execute("CREATE VIEW IF NOT EXISTS 'bh_" + channel + "_newestActive' AS SELECT * FROM 'bh_" + channel + "_bans' WHERE type='A' ORDER BY timeSet DESC LIMIT 1;");

  //The oldest active ban (for rotating out)
stat.execute("CREATE VIEW IF NOT EXISTS 'bh_" + channel + "_oldestActive' AS SELECT * FROM 'bh_" + channel + "_bans' WHERE type='A' ORDER BY timeSet ASC LIMIT 1;");
/*
 * EVIL SIDE EFFECT CODE ENDS
 */
```

Bronystate's channel has a limited number of concurrent user bans allowed
to be set. Normally, this limit is 50, given that the server must compare
all joins to the list, for hundreds of channels, tens of thousands of
users, and not explode the memory of each server in the network. Due to our
high traffic, and good standing with the IRCOps, our limit is 500. Once 
that limit is reached, new `+b` modes are outright rejected. This has been
an issue more than once in Bronystate's years of operation, with large spam
attacks demanding more bans be handed out than we have room for.

The proposal was simple. Build a bot that keeps its own list of bans, which
can be orders of magnitude longer than what the server will track, and
quietly rotate old bans off the channel to make room for new ones, while
making sure those bans still apply by checking joins against them, and
reapplying the ban if there's a match. The implementation was going to be
simpler. The bot *really* only needs to keep a list of these "inactive"
bans, the ones not set in channel but not forgotten, and check joins
against that, and this list would surely not be *that* big. We could even
have a statute of limitations; if a ban was "inactive" for a long period of
time, we could let the bot mark it as unbanned entirely, further shortening
the list it needs to track.

But then the requirements of the bot sunk in. This list of bans needed to
be kept in sync with the actual list applied on the server. If the bot
thinks a ban should be actively `+b`, but it isn't when it goes to look, it
must mark it as unbanned. If there's something in the list of bans that the
bot knows about, it must update that existing record to avoid duplication,
and make searching logs easier. And the operators should have full control;
the bot must mark unset any ban it explicitly sees `MODE -b` for.

Oh, and it needs to be able to track all this even after starting up fresh.

So it was time for me to finally learn databases, and I settled on SQLite
for the wonderful ease of a single compact file I could move from server
to desktop when testing. Just make a table of bans, a table of notes that
references those bans, and some options to configure the ban rotation logic
without using magic numbers in the code or needing a restart to apply. It
took several weeks of design to settle on how the setting or unsetting of a
ban would pass through `HikariBot` to the `Banhammer` component, how it
would handle grabbing the list of currently set bans from a channel, and
days of research and testing to even wire up a database to begin with.

Then came the triggers. Since an individual ban could have its state
changed in a number of ways --- scraped from the list and new, scraped and
was already in the database, missing from the list, set by an op, unset by
an op, inactivated to make room for a new ban, reactivated because someone
joined matching it --- I wanted to be sure that every change was properly
logged, alongside any notes the staff would make about the ban. This proved
much, *much* easier said than done, as it was impossible to implement it
all in the `Banhammer` Java code. Merging the list of bans scraped from the
channel required sending the whole list to the database. The alternative,
sending them one by one, caused a great deal of I/O blocking, and took
minutes for a few hundred entries. Given the list at once, the database
could do the merge in milliseconds, but at the expense of doing it all
silently; the Java code would have no indication of the results of a
scraped ban entering the database.

For that, I set up triggers to fire when the ban state changes. If it's
inserted anew by the scrape, it gets one type, if it was updated by the
scrape, it gets another. The triggers would catch these changes, make an
appropriate log entry, then set the state again to wherever it was going.
An inserted-by-scrape ban is on its way to being marked "active", a ban
missing from the scraped list passes through "missing" before becoming
"unset", and so on. Each of these triggers needs to be present for each
channel whose bans we track, as well as the tables for the bans themselves,
and HikariBot is designed to need as little extra help as possible when
starting up. If the database isn't there, she creates it, and if the tables
aren't there, she creates those too.

Unfortunately, [Hikari][abthik] started developing memory issues just as I
was making significant headway into finally implementing the actual logic
of rotating out bans to make room for new. The spaghetti of triggers had
eaten up several days of thought, and the entire project hit the backburner
when programs were suddenly and randomly segfaulting all over the server.
It sits now in the repository on its own branch, patiently awaiting the day
I return to finish building it. I have grand designs for a web-facing
interface to list the bans, their information, notes, and all, and of
course only accessible by the authorized Bronystate staff, but it will have
to wait.
