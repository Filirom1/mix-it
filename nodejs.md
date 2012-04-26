# Node.js

From transloadit.com

# History

Discovered node v0.0.6.
At this time not serious for production.

API was not fucked-up at the beginning.

Wrote a few things:
* node-mysql
* formidable

Everyone love NPM ;)

In 2009, about 2000 people in the mailing list

## Node what's is good and what's is not.

Ubber sclalable : Node + MongoDB

Not so true.

10 requests per seconds, you don't need to worry about scaling. But
this is what people do.

Myth:

Threads don't scale
Events loops do.
Not true and not relevant.

Understand your tool. And use it for the right job.

Node load file from disk, v8 compile and execute it.
listen allocate a file descriptor,
the event loops starts

In the event loop find the fd with activities.

If the web server has activity, add a new fd (the connexion), when this
fd has activity pass it to JS.

Nothing interrupt your JS code running.


# Concurrency model

Advantage :

* low memory
* simple compared to threads to do something fast/efficient

Vertical scalability

Adding more resources to a single node.
A node is a computer : CPU, GPU, mem, dsik, ...
Most people talk of CPU:

Node use very little CPU, V8 use the CPU. 
V8 has JIT compilation.


But node run a single CPU, no shared mem.

# But ...

DO I need a shared memory ?

if yes, don't use nodeJS and good luck :)

if no, use `fork` in node. You will have one process per core.

We need to communicate between processes : Redis, ZeroMQ are good
friends.

# GPU

No support in node
node-cuda exists

## Memory

No hard memory limit, > v8-3.7

JS is garbage collected language (avoid huge heaps)

Buffers do not count toward heap.

## Network

Node concurrency is optimized for networking

## Disk

Done in thread pool

sendFile not working yet.

## Horizontal scalability

Adding more nodes.

Node has no feature for this.
And it's really hard to do.

## Problems
de-coupling/encapsultaion
CAP theorem
Syetm automation


One tip

Monitor & Measure
Collect: node-measured, statsd
Analyze: Graphite, LIbrato Metrics
Debug


If node do not have magic for scaling, what's good for ?

## Example

Node.js is really great with pipes (fs, child_process)

But each time we do a request, we create a new process. It's not so
good. Let's use stream-cache.


