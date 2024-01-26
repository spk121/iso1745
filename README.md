123456789012345678901234567890123456789012345678901234567890123456789012

# ISO 1745

## Introduction

ISO 1745 (aka ECMA-16) is a methodology for communication between
between computers over shared 8-bit serial channel. It is now ancient
and little
used, but, it was interesting in that it provided communication,
control, addresing, and messaging between machines using a single
shared bidirectional communication channel.

To put it in modern terms, ISO 1745 was a protocol that described
a bus, an inter-processs communication mechanism that allows
communcation between multiple machines or multiple processes on
the same machine.

## Overview

ISO 1745 was system that required cooperation: each process on the bus
had to know when its role would allow it to communicate.  There were
Master and Slave roles; only one process at a time had the Master role, and
that role could be handed off to
different processes.  There was usually no controlling process that enforced 
compliance to the standards.  Each process had a responsibility to
adhere to the standards.  There might have been a controlling process on
a different bus, but, that is out of scope of the standard.

Each process on the bus had an "address", which was a string that
denoted the process.  But there was no central registry of
addresses.  Each process chose its own address, and address collisions
were possible.  Uniqueness of addresses was a problem beyond the scope
of the standard.  The communication was message-based, with 
each message having sub-sections denoted as HEADER and TEXT.
A message could have any number of HEADER and TEXT sections.

By default, the messages were strict ASCII, but there were escape
sequences that allowed 8-bit raw message data.

Processes were required to reply or to acknowledge messages within
a given time frame.

A basic CRC for each message was added to ensure message integrity.

### Phase 1: Switching and Identification

Each process somehow connects to the bus and registers its address
with the Bus Controller.

### Phase 2A: Establishment of Data Link: Polling

When idle, the Bus Controller will poll each process, giving the
process the opportunity to become a Master station.

The Bus Controller sends out the following message to a connected
process.

`   EOT + address + ENQ`

And then it starts the No Response timer.

If the process has no messages to send, it should respond with

`   (prefix) + EOT`

Prefix is optional info that could be sent to the Bus Controller.

If the process has messages to send, it should accept its
role as a Master station and send out selection
messages as described in the next section.

The process does not respond with either an `EOT` or a selection
message within the No Response Timeout, the Bus Controller
will poll the next process.

### Phase 2B: Connecting Master to Slave(s): Selecting

A process that has accepted the role of the Master process
must inform other processes that they are to be Slave
processes in the upcoming communication.

For each process with which the Master process desires
to communicate, it sends out

`   address + ENQ`

And starts a No Response timer.

Each prospective Slave process must respond with either

`   ACK`

or 

`   (prefix) + NAK`

within the given No Response Timeout.

If a prospective Slave process does not respond within the timeout,
the Master process may retry or give up.

The slave processes will then know that they are the intended
recipients of any following data messages.

### Phase 3: Information Transfer

Now the Master process may send out informational messages, initiated 
by `SOH` or `STX` and terminated with `ETX` or `ETB`.

The Slave process will respond with `ACK` in the affirmative case,
`(prefix) + NAK` if a retransmission is requested, or `EOT` if it
intends to disconnect.

### Phase 4: Termination

When the Master process has no more messages to send, it sends
`EOT`, thus relinquishing its Master status and returning control
to the Control process so that it can poll the next process.

Or, if the client had sent `EOT` instead of either `ACK` or `NAK`.
This Master/Slave connection is considered terminated.

If either Master or Slave sends `DLE+EOT`, they are disconncted
from the bus.

### The Code

I once wrote code to implement this protocol. I was going to
put it here, but, I never did.  It was an interesting experiment,
but, not that useful nowadays.
