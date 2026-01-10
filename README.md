🚀 eefp – Express Extended For Protocols

eefp is a lightweight utility that extends an existing Express app with built-in support for:

✅ WebSocket routes (WS)

✅ Server-Sent Events (SSE)

✅ Middleware support for both

✅ Client management utilities

It works like a plugin: you pass your Express app into eefp(app) and your app instantly gains new protocol features.

&nbsp;

📦 Installation

bash

Copy

npm install eefp

&nbsp;

⚡ Quick Usage

js

Copy

const express = require("express")

const eefp = require("eefp")



const app = express()

eefp(app)   // extend express with WS + SSE



app.get("/", (req, res) => {

&nbsp; res.send("HTTP Working")

})



app.listen(3000, () => {

&nbsp; console.log("Server running on port 3000")

})

&nbsp;

🌐 WebSocket Support

eefp adds WebSocket routing directly to your Express app.

➤ Create a WebSocket Route

js

Copy

app.ws("/ws", (req, ws) => {

&nbsp; ws.send("Connected!")



&nbsp; ws.on("message", msg => {

&nbsp;   console.log("Received:", msg.toString())

&nbsp;   ws.send("Echo: " + msg)

&nbsp; })

})

✔ Supports middleware just like Express routes.

js

Copy

app.ws("/secure",

&nbsp; (req, res, next) => {

&nbsp;   console.log("Middleware executed")

&nbsp;   next()

&nbsp; },

&nbsp; (req, ws) => {

&nbsp;   ws.send("Secure connection established")

&nbsp; }

)

&nbsp;

🧠 WebSocket Utilities

eefp exposes helpful WebSocket helpers on the app instance:

🔹 Get WebSocket Server

js

Copy

const wss = app.getWss()

&nbsp;

🔹 Get All Connected Clients

js

Copy

const clients = app.getAllClients()

console.log("Total clients:", clients.size)

&nbsp;

🔹 Close All Clients

js

Copy

app.closeAllClients(1000, "Server shutting down")

&nbsp;

&nbsp;

📡 Server-Sent Events (SSE) Support

eefp allows you to create SSE endpoints easily.

➤ Create an SSE Route

js

Copy

app.sse("/events", (req, res) => {

&nbsp; res.write("data: Connected to SSE\\n\\n")

})

Clients can connect using:

js

Copy

const es = new EventSource("/events")

es.onmessage = e => console.log(e.data)

&nbsp;

📢 Broadcast SSE Messages

Send a message to all connected SSE clients:

js

Copy

app.broadcastSse("Hello all clients!")

&nbsp;

👥 Get All SSE Clients

js

Copy

const clients = app.getAllSseClients()

console.log("SSE clients:", clients.size)

&nbsp;

&nbsp;

🔁 Router Support

If you are using Express Router, SSE routes also work:

js

Copy

const router = express.Router()



router.sse("/stream", (req, res) => {

&nbsp; res.write("data: Router SSE Connected\\n\\n")

})



app.use("/api", router)

&nbsp;

&nbsp;

🎯 How It Works

• 

eefp(app) internally:

• 

Adds WebSocket support using ws

• 

Adds SSE support using native HTTP streams

• 

Overrides app.listen() to attach protocol servers automatically

• 

Keeps track of connected clients

No extra configuration required.

&nbsp;

&nbsp;

📄 API Summary

WebSocket

Method

Description

app.ws(path, ...handlers) 

Create WebSocket route

app.getWss() 

Get WebSocketServer instance

app.getAllClients()

Get all connected WS clients

app.closeAllClients(code, msg) 

Close all WS clients

Copy table

&nbsp;

SSE

Method

Description

app.sse(path, ...handlers)

Create SSE route

app.broadcastSse(message) 

Send message to all SSE clients

app.getAllSseClients()

Get all connected SSE clients

Copy table

&nbsp;

&nbsp;

🛡️ Requirements

• 

Node.js 16+

• 

Express

• 

ws

