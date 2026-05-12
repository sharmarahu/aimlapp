# Real-Time Collaborative Rooms

AIML App supports multi-user AI chat rooms where every participant sees messages in real time. The AI participates in the conversation alongside the team.

---

## How It Works

```
User A (browser)  ──WebSocket──┐
User B (browser)  ──WebSocket──┤──► Durable Object (ChatRoom)
User C (browser)  ──WebSocket──┘         │
                                          ├── Broadcasts to all connected users
                                          ├── Persists messages to D1
                                          └── Delegates LLM requests to main worker
```

Real-time rooms are powered by **Cloudflare Durable Objects** — stateful workers that maintain a single, globally consistent WebSocket hub per room. There is no polling or long-polling involved.

---

## Starting a Room

Every user on AIML App is automatically placed in a room. If you visit `/app`, your room ID is generated and stored in `localStorage`. The URL updates to include `?room=room_abc123`.

### Sharing a Room
Copy the URL from your browser and send it to teammates. When they open it:
- If they're logged in, they join immediately
- If not, they see a sign-in prompt before joining

### Creating a New Room
Open the room management panel and click **New Room**. A fresh room ID is generated and you're redirected to it.

---

## Room Features

### Real-Time Sync
Messages from all participants appear instantly without page refresh. The WebSocket connection is maintained for the lifetime of the browser session with automatic reconnection and ping/keepalive.

### AI in the Room
Any participant can send a message to the AI. The AI response is broadcast to all connected participants simultaneously — everyone sees the answer at the same time.

### Typing Indicators
When a user is typing, other participants see a typing indicator with their name.

### Thread Replies
Reply to any specific message to keep conversations organized. Thread replies are anchored to their parent message visually.

### Reactions
React to any message with emoji. Reactions are synced to all participants instantly.

### @Mentions
Type `@` followed by a participant's name to mention them. Mentioned users receive an in-room highlight.

---

## Room Persistence

- Messages are persisted to Cloudflare D1 via a write-behind buffer in the Durable Object
- The full room history is loaded when a participant opens or rejoins a room
- Room history is paginated — the last 100 messages are loaded by default
- Rooms expire after **24 hours of inactivity** (`updated_at` based) — all messages are hard-deleted at that point

---

## Silent Standups

A special type of room for async team standups. See [Organizations & Multi-Tenancy](multi-tenancy.md#silent-standups) for the full workflow.

Key differences from a standard room:
- Created by an org admin via the API (not manually)
- All org members receive an email invite
- When the standup ends, an LLM generates Meeting Minutes from the transcript and emails them to all participants

---

## Technical Details

### WebSocket URL
```
wss://aimlapp-realtime.it-rahul1.workers.dev/ws
```

The realtime worker is a separate Cloudflare Worker deployment from the main app. It shares the main D1 database for message persistence and reads sessions from the same `SESSIONS_KV` for authentication.

### Connection Flow
```
1. Browser opens WebSocket connection to /ws?room=<roomId>&token=<sessionToken>
2. Durable Object validates the session token against SESSIONS_KV
3. If valid, user is registered as a participant in the room
4. All subsequent messages are broadcast to all registered WebSocket connections
5. LLM requests are forwarded to the main worker via APP_URL (HTTP)
6. LLM responses are broadcast to all participants when received
```

### Reconnection
The client automatically reconnects on connection loss with exponential backoff. A ping/pong keepalive runs every 30 seconds to detect stale connections.

### Message Deduplication
Messages are deduplicated by ID on the client side — if a message arrives via both WebSocket push and a poll/load, it is only displayed once.

---

## Limits

| Parameter | Value |
|---|---|
| Max participants per room | No hard limit (Durable Object scales per room) |
| Message history loaded on join | Last 100 messages |
| Room inactivity expiry | 24 hours |
| WebSocket keepalive interval | 30 seconds |
| Reconnection strategy | Automatic, exponential backoff |
