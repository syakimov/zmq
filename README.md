# ZMQ

Buzzwords {
  TCP socket
  latency
  scale
  networking
  distributed computing
  routing
  paralel
  connected
  arbitrary -> random
  upstream -> send data from client to server
  downstream != upstream
  gotcha -> valid but tricky
  load balancing -> distributing tasks to workers
  fair queuing -> collect results from workers evenly
  pipeline pattern
  data frame = 'tabular' data -> data like a row in SQL db
  wire protocol -> data transfer between apps
  interoperability -> how to make C and Go use same data structures
  interoperability -> interpret the info transferred between programs
  messaging layer
  SOAs -> service-oriented architectures
  topology -> structure of the app network
}

Buzz expressions {
  to bound to an endpoint
  to connect upstream to something
}

Beginning {

  Pieter Hintjens
  Fred Brooks

  embeddable networking library but acts like a concurrency framework

  sockets carry atomic messages

  transports
    in-process
    inter-process
    TCP
    multicast

  connect sockets N-to-N with patterns
    fan-out
    pub-sub
    task distribution
    request-reply

  clustered products

  asynchronous I/O model
  scalable multicore applications

  asynchronous message-processing tasks
}


Concept {
  zero broker -> zero latency
  simple
  tradeo-offs game
}

Basics {
  fast
  sockets
  routing
  small
  strong and scilent
  decentralised
  thread
  protocol
  network
  productivity
  reliability
  mainframe
  connected code
  IETF
  clients
  protocols
    UDP
    TCP
  websockets
  HTTP
  P2P architecture
  Skype Bittorent
  building block
}

Client-Server-Model {
}

RPC {
}

Strings {
  Protocol Buffer
  formatting data
  message frame
  ZeroMQ strings are length-specified and are sent on the wire without a trailing null
}

Getting the msg out {
  PUB - SUB pattern
  one way data distribution
  server push data to set of clients
  filter subscriptions
  asynchronous
  bind the PUB and connect the SUB
  Always subscribe before publish because you will lose msg
  TCP connection handshaking
  establish connection - 5 msec
  assume data stream is infinite and don't care about before

  Not understood {
    * A subscriber can connect to more than one publisher, using one connect call each time. Data will then arrive and be interleaved ("fair-queued") so that no single publisher drowns out the others.
    * If a publisher has no connected subscribers, then it will simply drop all messages.
    * If you're using TCP and a subscriber is slow, messages will queue up on the publisher. We'll look at how to protect publishers against this using the "high-water mark" later.
    * From ZeroMQ v3.x, filtering happens at the publisher side when using a connected protocol (tcp:// or ipc://). Using the epgm:// protocol, filtering happens at the subscriber side. In ZeroMQ v2.x, all filtering happened at the subscriber side.
  }
}

Divide and Conquer {
  paralel pipeline
  the worker `connect upstream` to the ventilator (PULL)
    and `downstream` to the sink                  (PUSH)
  ventilator and sink are `stable`
    while workers are `dynamic`
  ventilator is responsible for load balancing
  sink is responsible for fair queuing
  `slow joiner` syndrome ->
    if you do not synch the PULL workers connection
      some will get less workload than others.
}

Getting the Context Right {
  the context is the container for all sockets in a single process
  the context is responsible for the transport for inproc sockets
  inproc sockets are the fastest way to connect threads in one process.

  // create and destroy context
  {
    context, _ := zmq.NewContext()
    defer context.Close()
  }
}

Making a Clean Exit {
  Main objects:
    messages
    sockets
    contexts

  exit process
    close sockets
    close context
}

Why We Needed ZeroMQ {
  reusable message layer has to solve the following
    * I/O has to be in the background
    * dynamic components: servers and clients.
    * Server to server connection. How is reconnection performed.
    * Message entity. Data framing. Efficient for small and big data.
    * How to queue up messages while client is down and flush them when is up
    * Messages are discarded or saved. Store MSG in queues or db.
    * Where store message queue? Strategy for slow message reader and queue grow?
    * Lost messages handle. MSG consistency ensured with reliability layer. RL crash
    * Change ins network transport (TCP multicast - unicast, IPv6)
    * Send message to multiple peers (MSG routing) How peers send acknowledge
    * enforce encoding for data types
    * handling network errors

  `broker` concept
    addressing
    routing
    queuing

  `broker`
    reduces complexity in large networks
    often becomes bottleneck
    used for failover scheme

    ZeroMQ does {
      handle I/O asynch in background threads which do not lock
      components can disconnect and connect at any time
      queues messages automatically when needed
      when overfulled queues either throws messages or blocks senders
      transport is abstracted and can be changed any time
      handle slow/blocked readers safely according to pattern
      supports PUB-SUB and REQ-REPLY message routing patterns
      delivers whole mesages as they were sent
      messages are blobs and you decide how to represent data (Protobuf)
      handles network errors and retries
      uses less CPU
    }

  ZeroMQ is {
    socket-inspired API
      zmq_send()
      zmq_recv()
    central loop of the app is message processing
    app is composed of message processing tasks
      each task maps to a node
      nodes talk to each other using easily changed transports
    nodes
      two nodes in one process -> node is a thread
      two nodes in one box(machine) -> node is a process
      two nodes in one network -> node is a box
  }
}

Socket Scalability {

}

http://zguide.zeromq.org/page:all#Socket-Scalability

Chapter 2 - Sockets and Patterns {
  synchronize subscriber and publisher -> avoid losing data
}
