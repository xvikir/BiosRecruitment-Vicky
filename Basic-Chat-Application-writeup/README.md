# Socket Chat Application

A simple two-way chat application written in Python using the built-in `socket` module. This was built to understand how socket programming works — how two programs can establish a connection and exchange messages over a network.

## How it works

There are two programs — `server.py` and `client.py`.

The server starts first. It binds to an IP address (`127.0.0.1`) and a port (`65432`), then listens for an incoming connection. The client then connects to that same address and port. Once the connection is established, both sides can send and receive messages.

Messages are encoded into bytes before being sent over the socket and decoded back into strings on the receiving end. The chat continues in a loop until either side types `bye`, which closes the connection.

## Files

- `server.py` — binds to a port, waits for the client to connect, receives messages and sends replies
- `client.py` — connects to the server, sends messages and receives replies

## Running it

You need two terminals open at the same time.

**Terminal 1 — start the server first**
```bash
python server.py
```

**Terminal 2 — then run the client**
```bash
python client.py
```

Type a message in the client terminal and the server will receive it and reply back. Type `bye` on either side to end the chat.

## What I used

- Python 3
- `socket` module (built-in, no installs needed)
- TCP protocol — used because it guarantees messages are delivered in order and nothing gets lost
- `127.0.0.1` (localhost) — both programs run on the same machine so no internet is needed

## What I learned

- How to create a socket and the difference between server and client roles
- What `bind()`, `listen()`, `accept()` and `connect()` actually do
- Why you encode text to bytes before sending and decode it back after receiving
- The difference between `send()` and `sendall()` and why `sendall()` is safer
