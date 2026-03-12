# Socket Chat Application

A simple chat application built using Python's socket module. Two programs run at the same time — one acts as the server and the other as the client. They communicate over TCP on localhost.

## Files

- `server.py` — waits for a connection, receives messages, sends replies
- `client.py` — connects to the server, sends messages, receives replies

## How to Run

Open two terminals.

**Terminal 1**
```bash
python server.py
```

**Terminal 2**
```bash
python client.py
```

Type messages in the client terminal. The server reads them and replies back. Type `bye` to end the chat.

## How it works

The server binds to `127.0.0.1` on port `65432` and waits. The client connects to that same address and port. Once connected, they take turns sending and receiving messages using `sendall()` and `recv()`. Messages are encoded to bytes before sending and decoded back to strings on the other side.

## Requirements

- Python 3
- No external libraries needed
