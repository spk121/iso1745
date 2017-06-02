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

## The Library and Daemon

This library provides capability to pack and unpack ISO 1745 messages,
and to connect to and receive messages on a shared ISO 1745 bus.
It relieves the application from the communication and parsing task,
allowing it to run a callback-like model, acting when messages are
received.

In the default implementation, this package provides a simple
daemon to operate as a bus.
Processes may open a read/write channel to the daemon.  Any
messages written to the daemon will be immediately echoed to
all other clients. The daemon is lightweight and does little
validation. It is up to the clients to transmit valid messages.


