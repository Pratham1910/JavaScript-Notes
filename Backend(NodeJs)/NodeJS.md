
# 🖥️ Creating Server using HTTP

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.end("Hello world from India");
});

server.listen(3000, () => {
  console.log('Server Running on port 3000...');
});

```


# Express.js

**Light-weight, fast Node.js framework.**

```js
const express = require('express')
const app = express()
app.get('/home', (req, res)=>{
    res.send("Welcome to the Home page!");
    console.log("Running..");  
})
app.get('/about', (req, res)=>{
    res.send("Welcome to the About page!");
    console.log("Running..");  
})
app.listen(3000, ()=>{
    console.log("Server running at port 3000..");
})
```


# API's

