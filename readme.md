
# SSH for CHICKEN 

This is very much work in progress.

## Supported standards

### [SSH Transport Layer](https://tools.ietf.org/html/rfc4253)

- kex algorithms:                  curve25519-sha256@libssh.org
- server host key algorithms:      ssh-ed25519
- encryption algorithms:           chacha20-poly1305@openssh.com
- mac algorithms:                  hmac-sha2-256
- compression:                     none

This is the bulk of the hard work. The result of this is an encrypted
channel where you can send and receive SSH packets. These packets are
then used by the other protocols.

## [SSH Authentication Protocol](https://tools.ietf.org/html/rfc4252)

You probably have to do most of this yourself, but it's not that hard.

## [SSH Connection Protocol](https://tools.ietf.org/html/rfc4254)

You have to do this youself.


# TODO

## fix known bug: will never initiate key negitiation

[RFC4253s9](https://tools.ietf.org/html/rfc4253#section-9) says:

> It is RECOMMENDED that the keys be changed after each gigabyte of
> transmitted data or after each hour of connection time, whichever
> comes sooner.  However, since the re-exchange is a public key
> operation, it requires a fair amount of processing power and should
> not be performed too often.

minissh will currently never initiate a key exchange (but will respond
correctly to when the remote side initiates).

## fix known bug: respect window limits

if you run the dump example, you'll find the ssh client eventually
prints out this:

    channel 1: rcvd too much data 32768, win 0

this is known and need to be implemented. output and error ports
running under run-channel must block when their window-sizes aren't
big enough.

## fix known bug: limit ssh packet sizes to 32k as per specification

because `(display (make-string (* 1024 1024)))` fails, for example.

## plus these things

- everywhere: nice API
- make a ssh client too
- transport: allow querying current encryption level
- channels: pty handling?
- find a faster current-entropy-port
