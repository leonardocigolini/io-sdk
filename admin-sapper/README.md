``# io-sdk admin 


this document describe implementation of this app usign Sapper

original README was saved in sapper.md


date|author|notes
---|---|---
2020-07-19|lcg|first draft
2020-09-05|lcg|here i describe difference between sapper and svelte versions
2020-09-08|lcg|api.js

## introduction

this application provides 7 basic commands:

* Import from URL
* Custom Import
* Send Messages
* Send Single Message
* Debug
* Development
* About

the first version of this application, developed as a plain Svelte app, without Sapper, gives access to those functions through several routes, implemented using svelte-routes:

* import
* custom
* ship
* send
* debug
* editor
* about

each route is related to an external command or to a Svelte component, according to this table:

Menu command | Route | Component |Action
----|----|----|----
Import from URL|import| Import | util/import
Custom Import |custom|  Import | iosdk/import
Send Messages |ship| Ship |
Send Single Message |send|Send |
Debug|debug|Debug |
Development | editor || http://localhost:3000
About | about | About |
|/send?key|Send|
|/debug?key|DebugData

  

## Sapper implementation

Sapper provide built-in routes management, but the mechanism is a little different from svelte-routes.  

Each routes it’s not declared but is automatically detected from Sapper looking at the files and directories contained in src/routes directory. 

In order to activate a route is enough a link to a url containing the name of the route itself. 

If we need to associate different routes to the same component (like for example the component Import) with Svelte it’s possibile to pass props, but with Sapper we need to add query parameters to the url.

The command Development don’t require an associated route, it’s enough to specify the final destination.


Menu command | Route | Component |Action
----|----|----|----
Import from URL|/import?action=1| Import | util/import
Custom Import |/import?action=2|  Import | iosdk/import
Send Messages |/ship| Ship |
Send Single Message |/send|Send |
Debug| /debug|Debug |
Development |  http://localhost:3000||
About | /about | About |
|/send?key|Send|
|/debug?key|Debug


for this reason src/routes contains the following files/directories:

* import
* ship
* send
* debug
* about

some of this commands/components are connected to extra routes or depends on other components.

### Import

the Import component has several states connected to many different sub components:


condition | sub component
----|----
state.form | ImportForm |
state.error| - |
state.data && !isPreview | ImportData |
state.data && isPreview | ImportPreview |

### Ship

the Ship component uses /send route, with a slug to identify the msg to be sent. 

### Debug

Also the Debug component uses /debug route, with a slug to identify the msg to be sent. The Svelte version uses a DebugData component, the Sapper version uses directly the Debug component.


## application state


In order to keep the application state, a couple of stores are defined in /src/components/store.js:

* formData
* reload

formData is used by Import and ImportForm components

reload is used by Debug component

## api

External method are managed through api defined in src/node_modules/api

Every endpoint uses as base url `http://localhost:3280/api/v1/web/guest` 

Api | endpoint | used by components
---|--- | ---
msg.load | iosdk/import , custom/import | Import
msg.save | util/store | Import
cache.scan | util/cache?scan=message:* | Ship , Debug
cache.get | util/cache?get= | Ship
cache.del | util/cache?del= | Ship
cache.clean | util/cache?clean= | Debug










