+++
title = "erlport traps exists"
slug = "slug"
date = 2020-02-15
[taxonomies]
+++
One of the killer features of [BEAM](https://en.wikipedia.org/wiki/BEAM_(Erlang_virtual_machine)) 
languages such as Erlang & Elixir is [ports](http://erlang.org/doc/reference_manual/ports.html). 
Ports allow a BEAM process to communicate with an external OS process safely.

If one reads the port docs (link above) one finds that, when a port is spawned
it is linked to the BEAM process that spawned it (called the [connected process](http://erlang.org/doc/tutorial/c_port.html)) 
& that if the port exits, the external OS process exits as well (assuming it is well behaved).
This is behavior is useful because it means that if the connected process crashes
you won't leak resources by having a growing number of unmanaged external OS
processes. The design protects against resource leaks by default.

[Erlport](http://erlport.org/) is an Erlang library which provides seamless
communication between python or ruby programs and BEAM programs via ports & 
automatically converts between Erlang & external language data-structures. Erlport
makes it very easy to interact with a python or ruby program as if it were another BEAM process,
which can be very useful for leveraging libraries written in ruby or python,
without needing to stand up an entire web-server to expose it to the BEAM.

However, one surprising (to me) behavior I discovered is that [Erlport processes](https://github.com/hdima/erlport/blob/master/src/erlport.erl#L118)
[trap exits](http://erlang.org/doc/reference_manual/processes.html#receiving-exit-signals).
This means that by default, if the process which spawned an
Erlport process exits, the Erlport process, as well as the port & external python
or ruby OS process are left running. This can easily lead to a memory leak if
one is (for example) calling `:python.start()`, within an
[Task.Supervisor.async_stream_nolink](https://hexdocs.pm/elixir/Task.Supervisor.html#async_stream_nolink/6)
task, and not guaranteeing `:python.stop/1` will be called, expecting
the task exiting to guarantee that the Erlport process, port, and external OS
process will also exit.

Erlport is a fantastic library, just be sure your program guarantees that
`:python.stop/1` or `:ruby.stop/1` is called when when you need to kill the external
process so that you don't leak memory.
