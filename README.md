# Distributed Systems — DCA/UFRN

Coursework and experiments in distributed systems, covering RPC paradigms, gRPC with Protocol Buffers, and distributed publish-subscribe messaging patterns. All multi-service examples are containerized with Docker.

---

## Tech Stack

| Technology | Role |
|-----------|------|
| **Python 3** | Primary implementation language |
| **gRPC** | High-performance RPC framework (HTTP/2 transport) |
| **Protocol Buffers (proto3)** | Interface Definition Language (IDL) and binary serialization |
| **`grpcio` / `grpcio-tools`** | Python gRPC runtime and code generation |
| **Docker + Docker Compose** | Multi-container service orchestration |

---

## Project Structure

```
python-grpc-distributed-systems/
├── atividade2/
│   └── protobuf-example/      # Standalone Protocol Buffers serialization examples
├── rpc_variacoes/             # RPC communication pattern variations
│   ├── proto/                 # .proto service definitions
│   ├── gen/                   # Generated Python stubs (*_pb2.py, *_pb2_grpc.py)
│   ├── services/              # gRPC server implementations
│   ├── clients/               # gRPC client implementations
│   ├── docker/                # Dockerfiles per service
│   └── scripts/               # Helper scripts (code generation, startup)
└── twitter/                   # Distributed pub-sub simulation
    ├── src/                   # Publisher, subscriber, and broker implementations
    └── venv/                  # Python virtual environment
```

---

## Components

### atividade2 — Protocol Buffers

Demonstrates standalone Protobuf message definition, serialization, and deserialization without a full gRPC service. Useful for understanding the wire format independently of the RPC layer.

### rpc_variacoes — RPC Pattern Variations

Implements and contrasts different RPC communication styles using gRPC:

- **Unary RPC**: Single request → single response (standard call-and-return)
- **Server streaming**: Single request → stream of responses
- **Client streaming**: Stream of requests → single response
- **Bidirectional streaming**: Concurrent streams in both directions

Each pattern is defined in a `.proto` file, generated into Python stubs, and implemented in separate client and server modules.

### twitter — Pub-Sub Simulation

Simulates a distributed Twitter-like publish-subscribe system where:
- **Publishers** post messages to topics
- **Subscribers** register interest in topics and receive messages asynchronously
- Demonstrates decoupled communication, broker intermediation, and event-driven patterns

---

## Setup

### Python environment

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install grpcio grpcio-tools
```

### Regenerating gRPC stubs from .proto files

```bash
cd rpc_variacoes
python -m grpc_tools.protoc \
  -I proto/ \
  --python_out=gen/ \
  --grpc_python_out=gen/ \
  proto/*.proto
```

---

## Running

### rpc_variacoes (local)

Start the server first, then the client:

```bash
# Terminal 1
python services/server.py

# Terminal 2
python clients/client.py
```

### rpc_variacoes (Docker)

```bash
cd rpc_variacoes
docker compose up --build
```

### twitter pub-sub simulation

```bash
cd twitter
source venv/bin/activate

# Terminal 1 — broker/server
python src/broker.py

# Terminal 2 — subscriber
python src/subscriber.py

# Terminal 3 — publisher
python src/publisher.py
```
