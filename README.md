# 🎬 WatchTogether — Real-Time YouTube Watch Party

A full-stack, real-time YouTube Watch Party application that allows multiple users to watch videos together in synchronized rooms, communicate through live chat, and manage playback using role-based permissions.

Built as part of the **Web3Task Full Stack Developer Technical Evaluation**.

### 🌐 Live Demo

**[🚀 Launch WatchTogether](https://web3task-watch-party-psi.vercel.app/)**

**[💻 GitHub Repository](https://github.com/Kavya3004/web3task-watch-party)**

**[⚙️ Backend Health Check](https://web3task-watch-party-api.onrender.com/health)**

---

## ✨ Features

### 🎥 Real-Time Video Synchronization
- Synchronized YouTube video playback across connected users.
- Play, pause, and seek controls.
- Change the currently playing YouTube video.
- Broadcast playback updates through Socket.IO.
- New participants receive the room's current playback state.

### 🏠 Room Management
- Create private watch rooms with unique room codes.
- Join existing rooms using a room code.
- Share invitation links with other participants.
- View connected users in real time.
- Automatically assign the Host role to the room creator.

### 🔐 Role-Based Access Control

The application supports three user roles:

| Permission | Host | Moderator | Participant |
|---|---|---|---|
| Watch videos | ✅ | ✅ | ✅ |
| Live chat | ✅ | ✅ | ✅ |
| Play / Pause | ✅ | ✅ | ❌ |
| Seek video | ✅ | ✅ | ❌ |
| Change video | ✅ | ✅ | ❌ |
| Assign moderators | ✅ | ❌ | ❌ |
| Remove participants | ✅ | ❌ | ❌ |

**Security:** Playback and role-management permissions are validated on the backend, preventing unauthorized users from controlling room state through direct Socket.IO events.

### 💬 Live Chat
- Real-time room messaging.
- Participant names displayed alongside messages.
- Messages broadcast to connected room members.

### 📱 Responsive Interface
- Modern dark-themed UI.
- Responsive layouts for desktop and mobile screens.
- Participant management sidebar.
- Playback controls and video URL input.
- Live connection and participant indicators.

---

## 🛠️ Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript |
| Build Tool | Vite |
| Styling | Custom CSS |
| Backend | Node.js, Express |
| Real-Time Communication | Socket.IO |
| Video Integration | YouTube IFrame Player API |
| State Management | React Hooks, Server-Side In-Memory State |
| Frontend Hosting | Vercel |
| Backend Hosting | Render |
| Version Control | Git, GitHub |

---

## 🏗️ System Architecture

The application follows a client-server architecture with real-time bidirectional communication.

```text
              ┌──────────────────────────┐
              │       React Client       │
              │    YouTube IFrame API    │
              └────────────┬─────────────┘
                           │
                    Socket.IO Events
                           │
              ┌────────────▼─────────────┐
              │    Express + Socket.IO   │
              │                          │
              │  Room Management         │
              │  Permission Validation   │
              │  Playback State          │
              │  Participant Management  │
              └────────────┬─────────────┘
                           │
                   Broadcast Updates
                           │
              ┌────────────▼─────────────┐
              │    Connected Clients     │
              │  Synchronized Playback   │
              └──────────────────────────┘
```

### How Playback Synchronization Works

1. A Host or Moderator performs a playback action.
2. The frontend sends a `playback_action` event to the Socket.IO server.
3. The server verifies the user's room membership and role.
4. If authorized, the server updates the room's playback state.
5. The updated room state is broadcast to connected participants.
6. Each client applies the received state to its YouTube player.

This ensures that the backend remains the authoritative source of shared playback state.

### Example Playback Event

```typescript
socket.emit("playback_action", {
  action: "seek",
  currentTime: 120
});
```

### Example Server-Side Authorization

```typescript
function mayControl(role: Role) {
  return role === "host" || role === "moderator";
}
```

Unauthorized playback requests are rejected by the backend.

---

## 📡 Socket.IO Events

| Event | Direction | Purpose |
|---|---|---|
| `create_room` | Client → Server | Create a new watch room |
| `join_room` | Client → Server | Join an existing room |
| `room_state` | Server → Client | Synchronize room and playback state |
| `playback_action` | Client → Server | Play, pause, seek, or change video |
| `assign_role` | Client → Server | Promote or demote moderators |
| `remove_participant` | Client → Server | Remove a user from the room |
| `chat_message` | Bidirectional | Send and receive room messages |
| `operation_error` | Server → Client | Report invalid or unauthorized actions |
| `removed_from_room` | Server → Client | Notify a removed participant |

---

## 📁 Project Structure

```text
web3task-watch-party/
│
├── client/
│   ├── src/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   ├── styles.css
│   │   └── vite-env.d.ts
│   ├── index.html
│   ├── package.json
│   ├── tsconfig.json
│   └── vite.config.ts
│
├── server/
│   ├── src/
│   │   └── index.ts
│   ├── package.json
│   └── tsconfig.json
│
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20 or later
- npm
- Git
- Modern web browser

### 1. Clone the Repository

```bash
git clone https://github.com/Kavya3004/web3task-watch-party.git
cd web3task-watch-party
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the project root:

```env
PORT=4000
CLIENT_ORIGIN=http://localhost:5173
```

For a custom backend URL, create `client/.env`:

```env
VITE_SERVER_URL=http://localhost:4000
```

### 4. Start the Development Servers

```bash
npm run dev
```

The application will be available at:

**Frontend:** http://localhost:5173

**Backend:** http://localhost:4000

**Health Check:** http://localhost:4000/health

### 5. Build the Application

```bash
npm run build
```

This builds both the React frontend and the TypeScript backend.

---

## 🧪 Testing the Application

1. Open the application in your browser.
2. Enter a display name and create a watch room.
3. Copy the generated room code or invitation link.
4. Open another browser window or device.
5. Join the same room using a different display name.
6. Test synchronized video playback and seeking.
7. Send messages through the live chat.
8. Promote a participant to Moderator.
9. Verify Moderator playback permissions.
10. Verify that regular Participants cannot control shared playback.
11. Test removing a participant from the room.

---

## 🌍 Deployment

The frontend and backend are deployed independently.

### Frontend — Vercel

**Live URL:** https://web3task-watch-party-psi.vercel.app/

Production environment variable:

```env
VITE_SERVER_URL=https://web3task-watch-party-api.onrender.com
```

### Backend — Render

**Backend URL:** https://web3task-watch-party-api.onrender.com/

Production environment variable:

```env
CLIENT_ORIGIN=https://web3task-watch-party-psi.vercel.app
```

The backend uses CORS configuration to allow requests from the deployed frontend.

---

## ⚠️ Current Limitations

- Room and participant state is stored in server memory rather than a persistent database.
- Rooms are lost when the backend restarts.
- Room state is managed by a single backend instance.
- Playback synchronization is event-driven and may experience timing differences due to network latency.
- Browser autoplay restrictions may require user interaction before video playback.
- Chat history is not persisted after room termination.

These trade-offs keep the initial implementation focused on real-time collaboration, room management, and role-based permissions.

---

## 🔮 Future Improvements

- PostgreSQL database integration for persistent rooms and users.
- JWT-based authentication and user profiles.
- Redis adapter for multi-instance Socket.IO scaling.
- Improved playback drift correction.
- Persistent chat history.
- Host ownership transfer and reconnection recovery.
- Video queues and playlists.
- Emoji reactions.
- Automated integration and end-to-end tests.

---

## 👩‍💻 Author

**Kavya**

GitHub: [@Kavya3004](https://github.com/Kavya3004)

Project: Web3Task Full Stack Developer Technical Evaluation

---

## 📄 Project Summary

WatchTogether demonstrates full-stack development using React, TypeScript, Node.js, Express, and Socket.IO.

The project focuses on real-time state synchronization, server-side authorization, collaborative room management, and deployment of a full-stack application.

**Built to make watching together easier, wherever you are.** 🎬
