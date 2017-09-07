<p align="center">
  <a href="http://www.adjoint.io"><img src="https://www.adjoint.io/images/logo-small.png" width="250"/></a>
</p>

Nanochain
=========

A minimal viable blockchain to demonstrate simple blockchain principles.

Project goals:

* Well-documented.
* Low-line count.
* Low-dependencies.
* Using classic proof-of-work.
* Basic P2P using UDP chatter.
* Supports chain reconfiguration.
* No encryption or accounts.
* In-memory.

Enabling Multicast
------------------

Nanochain uses a UDP multicast gossip protocol for P2P networking. Before
running the program, make sure your localhost network interface has multicast
enabled. The simplest way to do this on Unix-like systems is:

```bash
$ sudo ifconfig lo multicast
```

Otherwise, the program with fail with `addMembership: failed (Unknown error -1)`
error from the `Multicast` module when attempting to add the node to the
multicast group. 

Usually, `lo` is the name of the local loopback (localhost) network interface on
modern Linux machines, but could differ depending on your machine and/or OS. To
check what your localhost network interface is, type `ifconfig`; The interface
prefixed by `lo`, e.g. `lo0` is usually the interface of interest. Simply
replace `lo` with the name of your localhost network interface in the command
above.

Running the Demo 
-----------------

Install the [Stack](https://docs.haskellstack.org/en/stable/README/) build system:

```bash
$ stack setup
$ stack build
$ stack exec nanochain
```

Ports 3000, 3001, and 3002 are opened for the initial nodes. In the browser go to:

```
localhost:3000/blocks
```

To mine blocks query:

```
localhost:3001/mineBlock
```

To see the current chain query:

```
localhost:3000/blocks
```

To add a new peer, use the `addPeer` endpoint followed by any open port: 

```
localhost:3000/addPeer/3003
```

to add `localhost:3003` as a peer.

Consensus (Proof Of Work)
-------------------------

Nanochain uses a simple proof of work (PoW) approach to chain concensus: To mine
a block on the chain, a node must compute a nonce such that that resulting hash
of the block being mined begins with a number of 0's equal to `round(ln(n))`
where `n` is the length of the current chain. For this PoW implementation the
average nonce computed is `16^n`, so when the length of the chain surpasses 12
(`round(ln(n)) == 4`) it begins to take several seconds to mine each block. As
`n` surpasses 23, mining a block could take well over 10 minutes. 


License
-------

Copyright 2017 Adjoint Inc

Released under Apache 2.0.
