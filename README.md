# SecureChat - Telegram Clone with E2E Encryption

A fully-featured Telegram-like messaging application with custom end-to-end encryption, peer-to-peer file transfers, group chat support, and anonymous authentication.

## Features

### Security
- **Custom E2E Encryption Protocol**: Combines ECDH key exchange with AES-GCM encryption
- **Anonymous Authentication**: No phone number, email, or personal info required - just a username and password
- **Message Signing**: HMAC-SHA256 signatures for message authenticity
- **Encrypted File Transfers**: Files are encrypted before P2P transfer

### Messaging
- **Real-time Chat**: WebSocket-based instant messaging
- **Offline Message Queue**: Messages are queued locally (IndexedDB) and synced when connection is restored
- **Typing Indicators**: Real-time typing status
- **Message Status**: Pending, sent, and delivered status icons
- **Search**: Filter conversations by name

### File Transfers
- **P2P File Sharing**: Direct peer-to-peer file transfers using WebRTC Data Channels
- **Drag & Drop**: Simple file upload interface
- **Progress Tracking**: Real-time transfer progress indicators
- **Auto Download**: Received files are automatically downloaded

### Group Chats
- **Create Groups**: Add multiple members to a group conversation
- **Group Messaging**: Encrypted messages broadcast to all group members
- **Group Info**: View group members and details

### UI/UX
- **Responsive Design**: Works on desktop and mobile devices
- **Lightweight Interface**: Clean, minimal design
- **Toast Notifications**: User-friendly feedback messages
- **Presence Indicators**: Online/offline status

## Architecture

### Server (Node.js/Express)
- REST API for authentication, messages, groups
- WebSocket server for real-time communication and P2P signaling
- SQLite database for persistent storage

### Client (Vanilla JS + Vite)
- Custom E2E encryption module
- Offline message queue with IndexedDB
- P2P file transfer using WebRTC
- Responsive UI with modern CSS

## Encryption Details

### Protocol Flow
1. **Key Generation**: Each user generates an ECDH P-256 key pair on registration
2. **Session Setup**: For each conversation, a new session key pair is generated
3. **Key Exchange**: Public keys are exchanged via the signaling server
4. **Shared Secret**: ECDH derives a shared secret between peers
5. **Encryption**: AES-256-GCM encrypts all messages
6. **Authentication**: HMAC-SHA256 signs messages for integrity

### Security Properties
- **Forward Secrecy**: New session keys per conversation
- **Message Integrity**: HMAC signatures prevent tampering
- **Confidentiality**: AES-GCM provides authenticated encryption
- **No Server Access**: Server only handles encrypted payloads

## Installation

### Prerequisites
- Node.js 16+
- npm

### Setup
```bash
# Clone the repository
git clone <your-repo-url>
cd telegram-clone

# Install server dependencies
npm install

# Install client dependencies
cd client
npm install

# Build the client
npm run build

# Start the server
cd ..
npm start
```

### Development
```bash
# Start server in development mode
npm run dev:server

# Start client in development mode (in another terminal)
cd client
npm run dev
```

## Usage

1. Open the application in your browser (http://localhost:3000)
2. Register a new account with just a username and password
3. Login with your credentials
4. Start a conversation by selecting a user from the list
5. Send encrypted messages in real-time
6. Create groups for multi-user conversations
7. Transfer files directly using P2P connections

## Project Structure

```
telegram-clone/
├── package.json
├── server/
│   └── index.js          # Express server with WebSocket
├── client/
│   ├── package.json
│   ├── vite.config.js
│   ├── index.html
│   ├── styles/
│   │   └── main.css      # Application styles
│   └── src/
│       ├── main.js       # Main application logic
│       ├── crypto.js     # E2E encryption protocol
│       ├── offlineQueue.js # Offline message queue
│       └── p2pTransfer.js # P2P file transfer
└── README.md
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Create new account
- `POST /api/auth/login` - Login to existing account

### Messages
- `GET /api/messages/:conversationId` - Get conversation messages
- `GET /api/conversations/:userId` - Get user's conversations

### Groups
- `POST /api/groups` - Create a new group
- `GET /api/groups/:userId` - Get user's groups
- `GET /api/groups/:groupId/members` - Get group members

### Users
- `GET /api/users` - Get all users
- `GET /api/users/online` - Get online users

### Offline Queue
- `GET /api/offline-queue/:userId` - Get and clear offline messages

## WebSocket Messages

- `auth` - Authenticate with server
- `message` - Send/receive encrypted messages
- `message_ack` - Message delivery confirmation
- `p2p_signal` - WebRTC signaling for P2P connections
- `typing` - Typing indicator
- `presence` - Online/offline status

## Security Considerations

- In production, replace the simple base64 password encoding with bcrypt
- Add rate limiting to prevent brute force attacks
- Implement proper CORS policies
- Use HTTPS/WSS in production
- Consider adding Perfect Forward Secrecy with ratcheting

## License

MIT License - Feel free to use and modify for your own projects.
