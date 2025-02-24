# Web Frontend_Backend MERN Project concurrently 3-1

npm init -y

https://www.npmjs.com/package/concurrently

npm i concurrently

create .gitignore

node_modules


"dev": "concurrently \"npm run server\" \"npm run client\"",
    "server": "npm run dev --prefix server",
    "client": "npm run dev --prefix client"
===================================
# Web Frontend_Backend MERN Project  axios 3-2

- create utils folder
- create index.jsx file


https://www.npmjs.com/package/axios

npm i axios

>> index.jsx

import axios from 'axios';

const productionUrl = 'http://localhost:5000/api';

export const customFetch = axios.create({
  baseURL: productionUrl,
});






