# Multithreaded-Programming
## NodeJS
Node.js is a single threaded language which in background uses multiple threads to execute asynchronous code.
Node.js is non-blocking which means that all functions ( callbacks ) are delegated to the event loop and they are ( or can be ) executed by different threads. That is handled by Node.js run-time.

- Node.js does support forking multiple processes ( which are executed on different cores ).
- It is important to know that state is not shared between master and forked process.
- We can pass messages to forked process ( which is different script ) and to master process from forked process with function send.

## Why and when we need to fork another process?
- Forking multiple processes is essential for freeing up memory and unloading single process.
- When we need to delegate tasks ( run them in parallel ) to another process for the sake of the speed.

Let’s look at this example:
We have REST endpoint which needs to call long running function in the body:
`server.js`

```js
const { fork } = require('child_process');
const express = require('express')
const app = express()

app.get('/endpoint', (request, response) => {
   // fork another process
   const process = fork('./send_mail.js');
   const mails = request.body.emails;
   // send list of e-mails to forked process
   process.send({ mails });
   // listen for messages from forked process
   process.on('message', (message) => {
     log.info(`Number of mails sent ${message.counter}`);
   });
   return response.json({ status: true, sent: true });
});
```

`send_mail.js`

```js
async function sendMultipleMails(mails) {
   let sendMails = 0;
   // logic for
   // sending multiple mails
   return sendMails;
}
// receive message from master process
process.on('message', async (message) => {
  const numberOfMailsSend = await sendMultipleMails(message.mails); 
  
  // send response to master process
  process.send({ counter: numberOfMailsSend });
});
```

In this simple example we have demonstrated how you can send data to the forked process and how you can send it back.

## Python

## Java

## Go