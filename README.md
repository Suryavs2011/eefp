🚀 eefp – Express Extended For Protocols

eefp is a lightweight utility that extends an existing Express app with built-in support for:

✅ WebSocket routes (WS)

✅ Server-Sent Events (SSE)

✅ Middleware support for both

✅ Client management utilities

It works like a plugin: pass your Express app into eefp(app) and your app instantly gains new protocol features.

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

➤ Create WebSocket Routes

js

Copy

// Exact route

app.ws("/ws", (ws, req) => {

&nbsp; ws.send("Connected!")

})



// Parameterized route

app.ws("/chat/:roomId", (ws, req) => {

&nbsp; console.log("Room ID:", req.params.roomId)

&nbsp; ws.send(`Welcome to room ${req.params.roomId}`)

})



// Wildcard route

app.ws("/news/\*", (ws) => {

&nbsp; ws.send("This matches any /news/... path")

})

➤ Middleware Support

js

Copy

app.ws("/secure",

&nbsp; (req, res, next) => {

&nbsp;   console.log("Middleware executed")

&nbsp;   next()

&nbsp; },

&nbsp; (ws, req) => {

&nbsp;   ws.send("Secure connection established")

&nbsp; }

)

➤ Error Handling

Middleware or handler errors are automatically propagated to Express. Handle them like normal Express errors:

js

Copy

app.use((err, req, res, next) => {

&nbsp; console.error("WebSocket error:", err)

})

&nbsp;

🧠 WebSocket Utilities

eefp exposes helpful WebSocket helpers:

🔹 Get WebSocket Server

js

Copy

const wss = app.getWss()

🔹 Get All Connected Clients

js

Copy

const clients = app.getAllClients()

console.log("Total clients:", clients.size)

🔹 Close All Clients

js

Copy

app.closeAllClients(1000, "Server shutting down")

&nbsp;

📡 Server-Sent Events (SSE) Support

eefp makes SSE endpoints easy.

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

➤ Broadcast SSE Messages

js

Copy

app.broadcastSse("Hello all clients!")

👥 Get All SSE Clients

js

Copy

const clients = app.getAllSseClients()

console.log("SSE clients:", clients.size)

&nbsp;

🔁 Router Support

eefp works with Express Routers for both WebSocket and SSE.

➤ WebSocket Routes in Router

js

Copy

const express = require("express")

const router = express.Router()



// Parameterized WS route

router.ws("/room/:roomId", (ws, req) => {

&nbsp; console.log("Room ID:", req.params.roomId)

&nbsp; ws.send(`Welcome to room ${req.params.roomId}`)

})



// WS route with middleware

router.ws("/secure",

&nbsp; (req, res, next) => {

&nbsp;   console.log("Router middleware executed")

&nbsp;   next()

&nbsp; },

&nbsp; (ws, req) => {

&nbsp;   ws.send("Secure WS connection established in router")

&nbsp; }

)



app.use("/api", router)

// Now WS routes are /api/room/:roomId and /api/secure

➤ SSE Routes in Router

js

Copy

const router = express.Router()



router.sse("/stream", (req, res) => {

&nbsp; res.write("data: Router SSE Connected\\n\\n")

})



app.use("/api", router)

// SSE endpoint is /api/stream

Notes:

• 

Route parameters (:param) and wildcards (\*) work inside routers.

• 

Middleware in routers executes for WS and SSE routes.

• 

Express-style error handling works across routers.

&nbsp;

🎯 How It Works

eefp(app) internally:

• 

Adds WebSocket support using ws

• 

Adds SSE support using native HTTP streams

• 

Overrides app.listen() to attach protocol servers automatically

• 

Keeps track of connected clients

• 

Supports advanced WS routing with parameters, wildcards, middleware, and Express-style error handling

No extra configuration required.

&nbsp;

📄 API Summary

WebSocket

Method

Description

app.ws(path, ...handlers) 

Create WebSocket route (supports middleware, params, wildcards)

app.getWss()

Get WebSocketServer instance

app.getAllClients()

Get all connected WS clients

app.closeAllClients(code, msg)

Close all WS clients

Copy table

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



🛡️ Requirements

• 

Node.js 16+

• 

Express

• 

ws

