# ZMQ

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

Divide and Conquer { <= To Be Con...
  parallel pipeline
}

Chapter 2 - Sockets and Patterns {
  synchronize subscriber and publisher -> avoid losing data
}
