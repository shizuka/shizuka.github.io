# Site Data

## projects.yml

Listing of working projects

### Format

~~~
- id: project
  name: Project Name
  desc: |
    Project description
  url: /path/to/project
  nolink: false
  noicon: false
  noborder: false
  
- id: another-project
  ...
~~~

### Required Fields
 - **id**
  - slug identifier for project
  - used for metadata defaults
 - **name**
  - friendly project name
  - used on project index (project page must define its own title)
 - **desc**
  - short description of project
  - used on project index

### Optional Fields
 - **url**
  - override project link
  - *default*: `/projects/[id].html`
 - **nolink**
  - project will have no link
  - *default*: false
 - **noicon**
  - project will have no icon
  - *default*: false, loads icon from `/projects/icon-[id].html`
 - **noborder**
  - project icon will have no 2px border + 2px padding as with other images
  - *default*: false
