To send SSE (Server-Sent Events) from anywhere in the app using Hono, you need to decouple the event stream management from the route handler. You can achieve this by storing active SSE connections and broadcasting messages when needed.

Here’s how you can do it:

Steps:
Maintain a list of active SSE connections.
Provide a way to send messages to all connected clients from anywhere.
Handle connection cleanup when clients disconnect.
Updated Code: Broadcasting SSE Messages Globally
typescript
Copy
Edit
import { Hono } from 'hono'
import { streamSSE } from '@hono/sse'

const app = new Hono()

let id = 0
const clients: Set<any> = new Set()

// SSE Route
app.get('/sse', async (c) => {
  return streamSSE(c, async (stream) => {
    clients.add(stream)

    // Cleanup when client disconnects
    stream.onAbort(() => {
      clients.delete(stream)
    })
  })
})

// Function to send SSE from anywhere
const sendSSE = (data: string, event = 'message') => {
  for (const client of clients) {
    client.writeSSE({
      data,
      event,
      id: String(id++),
    })
  }
}

// Example route to trigger SSE
app.get('/send', (c) => {
  sendSSE(`Triggered at ${new Date().toISOString()}`, 'custom-event')
  return c.text('SSE message sent!')
})

// Simulating a periodic event
setInterval(() => {
  sendSSE(`It is ${new Date().toISOString()}`, 'time-update')
}, 1000)

export default app
How It Works:
/sse route establishes a persistent connection and stores each client in a Set.
sendSSE(data, event) allows you to send messages from anywhere.
/send endpoint demonstrates how you can trigger SSE manually.
A setInterval function periodically sends time updates.
Now, you can send SSE messages globally by calling sendSSE('Your message', 'event-name') from any part of your app. 🚀
