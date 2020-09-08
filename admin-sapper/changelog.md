# io-sdk admin changelog

##  8.9.2020

###framework update:
* "sapper": "^0.28.0"
* "svelte": "^3.17.3"

### ref api.js

moved from /src/components to  /src/node_modules/api

api's are organized as modules (api.msg, api.cache)

api/index.js gives visibility to api modules, now you write: 

      import * as api from 'api';
      const msgs  = await api.import.load();

## 28.7.2020

many changes:

*  new src/components/api.js, access to external endpoint are centralized

## 22.7.2020

first commit

