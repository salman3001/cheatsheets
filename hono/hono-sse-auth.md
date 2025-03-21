To add authentication and send SSE messages to specific users, you need:

Authentication – Validate the user using a token (e.g., JWT).
Storing SSE Connections Per User – Maintain a map of users and their SSE connections.
Sending Messages to a Specific User – Lookup the user’s connection and send messages only to them.
Updated Code: SSE with Auth & User-Specific Messaging
typescript
Copy
Edit
import { Hono } from 'hono'
import { streamSSE } from '@hono/sse'
import { verify } from 'hono/jwt'

const app = new Hono()

const userStreams = new Map<string, any>() // Store SSE connections per user
let id = 0

// Middleware: Authenticate using JWT
app.use('/sse', async (c, next) => {
  const authHeader = c.req.header('Authorization')
  if (!authHeader) return c.text('Unauthorized', 401)

  try {
    const token = authHeader.replace('Bearer ', '')
    const payload = await verify(token, 'your-secret-key') // Replace with your JWT secret
    c.set('user', payload) // Attach user info to context
    await next()
  } catch (err) {
    return c.text('Invalid token', 401)
  }
})

// SSE Route (Authenticated)
app.get('/sse', async (c) => {
  const user = c.get('user') as { id: string; name: string }
  if (!user || !user.id) return c.text('Unauthorized', 401)

  return streamSSE(c, async (stream) => {
    userStreams.set(user.id, stream) // Store user's connection

    stream.onAbort(() => {
      userStreams.delete(user.id) // Cleanup on disconnect
    })
  })
})

// Function to send SSE to a specific user
const sendSSEToUser = (userId: string, data: string, event = 'message') => {
  const stream = userStreams.get(userId)
  if (stream) {
    stream.writeSSE({
      data,
      event,
      id: String(id++),
    })
  }
}

// Example route to trigger SSE for a specific user
app.get('/send/:userId', async (c) => {
  const userId = c.req.param('userId')
  sendSSEToUser(userId, `Hello User ${userId}, message sent at ${new Date().toISOString()}`, 'personal-message')
  return c.text(`SSE message sent to user ${userId}!`)
})

export default app
How It Works:
JWT Authentication Middleware (/sse route)

Extracts the token from Authorization header.
Verifies the token using hono/jwt.
Attaches user info (c.set('user', payload)) to the request.
Handling SSE Connections

Stores each user’s connection in userStreams (Map of userId -> SSE stream).
Removes connection when the user disconnects.
Sending Messages to a Specific User

sendSSEToUser(userId, data, event) finds the user’s SSE stream and sends a message.
Manual Trigger Example (/send/:userId)

Visit /send/123 (replace 123 with a real user ID) to send a message.
Client-Side Example (Connecting with Auth)
javascript
Copy
Edit
const token = 'your-jwt-token';
const eventSource = new EventSource('/sse', { headers: { Authorization: `Bearer ${token}` } });

eventSource.addEventListener('personal-message', (event) => {
  console.log('Received personal message:', event.data);
});
Final Thoughts
✅ Authenticated SSE using JWT
✅ User-Specific Messaging with Map<userId, SSE Stream>
✅ Secure & Scalable

Now, you can push real-time messages to authenticated users dynamically! 🚀
