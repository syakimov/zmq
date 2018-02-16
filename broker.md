the msg producer needs to create retrial logic
  1. add message in memory queue (it has to be fast)
  2. persist message in db (restore state in case of a crash)
  3. resend message until acknowledge from receiver
  4. remove message after acknowledge from receiver

implementation
  1. append messages to file
  2. append last send messages (the ones that are out of the queue)

The peer to peer communication
  it depends on the network bandwidth
    the message producer is limited by a window (lets say 10 msg per msec)
    the message producer sends the first 10 msg at instant and
      for every next msg he has to wait until he receives acknowledge

  to be scheduled for sending every msg is added in memory queue
    this queue is responsible for retry logic if no acknowledge is received
  after sending every message is added to waiting acknowledge queue
  every msg is persisted in case of a crash
  ater receiving acknowledge from the receiver the message is
    removed from the queue
    marked as send (this can be done in the same record or in another append only table)

HTTP2
  makes faster
  asynch
