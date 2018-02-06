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
  parallel pipeline
  the worker `connect upstream` to the ventilator (PULL)
    and `downstream` to the sink                  (PUSH)
  ventilator and sink are stable
    while workers are dynamic
}

Chapter 2 - Sockets and Patterns {
  synchronize subscriber and publisher -> avoid losing data
}
