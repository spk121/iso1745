123456789012345678901234567890123456789012345678901234567890123456789012

ISO 1745 (aka ECMA-16) is a methodology for communication between
between computers over shared 8-bit serial channel. It is now ancient
and little
used, but, it was interesting in that it provided communication,
control, addresing, and messaging between machines using a single
shared bidirectional communication channel.

It was system that required cooperation: each station on the channel
was only allowed to communicate when given authorization.  There were
Master and Slave roles; the Master role could be transferred to
different stations in a controlled manner.

Each station on a channel had an "address", which was a string that
denoted the station.  The communication was message-based, with 
each message having sub-sections denoted as HEADER and TEXT.
A message could have any number of HEADER and TEXT sections.

By default, the messages were strict ASCII, but there were escape
sequences that allowed 8-bit raw message data.

Stations were required to reply or to acknowledge messages within
a given time frame.

A basic CRC for each message was added to ensure message integrity.

This library provides capability to pack and unpack ISO1745 messages,
and to connect to and receive messages on a shared ISO1745 channel.
It relieves the application from the communication and parsing task,
allowing it to run a callback-like model, acting when messages are
received.
