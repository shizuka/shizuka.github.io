---
title: Vortex95 Mechanics
date: 2015-05-25 00:01
library: blog
---

# VoRTeX Mechanics

> A slightly abridged version of the help file included with Vortex95.

-----

## Rules of the Game {#rules}

 * Intended for ages three and up, or really anyone that knows
   how the pointy clicky thingy works.
 * 1-4 players supported, with 2 players and 2 AI providing "maximum
   enjoyment". In practice, 1v3 works just as well; it's all a mess when
   that many pieces are bumping each other around.
 * Duration could be less than a minute or over an hour, depending on the
   number of players, luck of the roll, and time taken deciding. The more
   players and higher the range of rolls, the longer it takes. A d12 sounds
   like a good idea, until all the pieces are crammed within 5 cells of the
   center, and everyone is skipping turns left and right.
 * Starting player is chosen randomly, then counterclockwise around the
   player starts on the board. All players get an equal number of turns
   before the game ends, so the starting player getting all tokens to the
   center isn't an instant game-over; the other players can catch up and
   ruin the winner's high score.
 * One token per space, except the safe corner spaces on the square board.
 * If a token lands on a space occupied by a rival token, that token is
   sent back to start (the player's corner). Except safe spaces, all four
   players could have a token in the same safe space.
 * You can't have two of your own tokens in the same space, safe or not.
 * You must roll exactly to put a token in the center. A token five cells
   away from the center must roll 5 to land there.
 * Getting all four tokens into the center wins the game, but does not yet
   end the game.
 * The game ends when a player has all four tokens in the center, and all
   players have taken an equal number of total turns. Two players could
   have all four tokens in the center this way. This is a tie.

-----

## Scoring {#scoring}

> Vortex has a strange scoring system, combining a total point value for the
> distance a player's tokens have extended on the board, and the distance
> *other* players' tokens have extended. In effect, if your tokens are near
> the center, and everyone else is at start, you have a very high score. If
> everyone is near the center, the score gap is smaller.

The score is a measure of how far a player has advanced her/his pieces and
the vulnerability of these pieces to be bumped by the rival pieces.

At the end of the game the winnners point total is calculated by adding the
differences between the winners score and the others. For example, if the
winner scored 90 points and the other three players scored 70, 40 and 50
the winners point total is calculated as follows.

```
Winner's points = { (90-70) + (90-40) + (90-50) } * 10
                = { 20 + 50 + 40 } * 10
                = { 110} * 10
                =  1100 pts
```

You need not understand the scoring method to play and enjoy the game. 
This information is given for players with an interest in probability and
risk analysis.

### The Algorithm

> The text in this section is rather confusing. The idea is that a player's
> total points is a combination of the total distance her tokens have
> covered, minus a value corresponding to the likelihood an opponent could
> bump one of her tokens. This algorithm is used by the AI to choose a
> token after rolling, by determining the possible point gain or loss for
> all pieces on the board.
> This algorithm assumes we're using a d6 for rolls.

1) For each stopping position away from start, ten points are awarded. For
   example, at the start of a game, if RED moves a piece by 6 spaces, then
   his score is 60.
   
2) If instead of being at the start of the game, assume this move from home
   was made during the middle of the game. Also imagine that there is an 
   opponents piece (Blue) that is less than six spaces (say four spaces)
   away from a Red piece. It is Blues turn to move and the probability that
   blue is likely to get a four is 1/6. Red is vulnerable and a throw of 4
   by Blue  could lead to Blue bumping the Red piece to starting position
   (where Reds score is zero). In other words, Red could lose 60 points, if
   Blue throws a four. The probability of Blue throwing a four is 1/6. In
   other word the probability of Red losing 60 points is 1/6. This could be
   neatly summarized into the following score for Red.

```
Reds score = (Actual position) - (Actual position * 1/6)
           = (60)              - (60 * 1/6)
           = (60)              - (10)
           = 50
```

3) If in addition to Blue piece 4 spaces away, there was another Blue piece
   5 spaces away, then the Red piece is vulnerable to a throw of either 4
   or 5 by Blue. The probability of throwing a 4 or a 5 is 2/6. This
   situation can be neatly summarized as follows

```
Reds score = (60)              - (60 * 2/6)
           = (60)              - (20)
           = 40
```

4) The same could be expanded to the Red piece being vulnerable to more
   than two pieces. 

5) If in the middle of a game, a player is faced with a difficult choice of
   having to pick from more than one possible move, a scoring system such
   as the one described above can lead to a logical decision that maximizes
   ones own advantage.

-----

## The Algorithm, written better

> A better-worded explanation of the scoring system

Each player has four tokens. On a 5x5 grid, a token can be at most 25 cells
away from start, for a maximum per-token score of 250, and a maximum score
overall of 1000 from the player's tokens alone. From this, we subtract a
vulnerability component, consisting of the probability of any of the other
players' tokens bumping one of ours.

A token in the center always contributes 250, and cannot be bumped.

Let's assume two players, RED and BLUE, and using a d6 for rolls. With the
5x5 board, the players start at opposite corners, so they are separated by
8 cells, exclusive, from each other's first-cell safe square. Five cells,
plus three from the next edge before the opposing safe.

RED has a token 18 cells into the board, BLUE has a token 5 cells into the
board. Because RED starts out 8 cells behind BLUE, RED's token is only
ahead 5 cells from BLUE. It's still much further along, of course, but BLUE
has less total distance to cover to ruin RED's day.

RED's token is worth **180** points to start (18 cells x 10 points per
cell). But BLUE's token is 5 cells behind, and with a d6, BLUE has a 1/6
chance of bumping RED's token, which would lose all 180 points. Thus, the
token is only worth `(180) - (180 * 1/6)` or `(180) - (30)` or **150**
points. Since this is RED's only token in the field, RED has **150**
points.

BLUE's token is worth **50** points, being 5 cells into the board. Relative
to RED, though, BLUE's token is (5 + 8) *13* cells away, meaning RED can't
bump that token, but this is moot. Five cells into the board, for all
players, is a safe square (safe at 1, 5, 9, and 13), so BLUE's token
couldn't be bumped anyway. BLUE, therefore, has **50** points.

-----

## Another Algorithm example

Lets throw CYAN into the mix, starting between RED and BLUE. Lets also use
a d12 for rolls instead of a d6. It's still a 5x5 grid, RED is 8 cells
behind BLUE, CYAN is 4 cells behind, and vice versa, for all relative
distances into the board.

All three players have a token 3 cells into the board, putting them in the
middle of the edge each player starts on. This would lead to the following
point calculations.

RED's token is worth **30** points. BLUE's token is 8 cells behind (BLUE 
will wrap through RED and CYAN's edges before spiraling in), so there's a
1/12 chance BLUE will bump RED's token. `(30) - (30 * 1/12)` or
`(30) - (2.5)`, or about **27**. But that's not all. CYAN's token,
having to also loop through BLUE and RED's edges before spiraling, is 12
cells away. Thus, CYAN *also* has a 1/12 chance of bumping RED's token, so
the token's value drops to `(30) - (30 * 2/12)` or **25**. Not only *that*,
but BLUE could move a brand new token out from start, as 3 cells in from
RED is only 11 cells in from BLUE. So RED has a total score of **23**
points from their one token, `(30) - (30 * 3/12)`, `30 - 7.5`.

CYAN suffers a similar problem. RED's token is 4 cells behind, BLUE is 12
cells behind, and RED could send a brand new token out 7 cells to bump. So
CYAN's token is likewise only worth **23** points.

BLUE has a worse problem. Not only is RED only 8 cells behind, and CYAN 4
cells behind, but *both* of them could send new tokens to bump BLUE's token
3 cells into the board. From **30** points, the token loses `30 * 4/12` or
10 points, giving BLUE a score of **20** points, even though it's exactly
as far into the board as RED and CYAN.

-----

## Victory

The final score is given by adding the difference of the winner's score
from the other players.

If RED were to instantly win in the above case, then RED would take
`((23 - 23) + (23-20)) * 10`, `3 * 10`, **30** total points. This would be
rather low, except we've shortcut a victory when RED should be starting
from 1000 points (25 cells to center, so 250 points per center token, and
four center tokens to win), and then subtracting the distance-minus-
vulnerability scores the other players reached.
