Lightning Talks
===============

Concurrency in JRuby
--------------------

### Why JRuby > Ruby in some cases?

* Working with Oracle databases (JDBC)
* RabbitMQ library - not crappy & officially supported

### 4 Rules of Concurrency in JRuby <small>[[source]](https://github.com/jruby/jruby/wiki/Concurrency-in-jruby)</small>

0. Don't do it.
0. If you must do it, don't share data across threads.
0. If you must share data across threads, don't share mutable data.
0. If you must share mutable data across threads, synchronize access to that data.

### Nuances of JRuby Concurrency

#### Thread Safety & Atomicity

*Safety first!*

Assumptions:

* assume standard Ruby data structures are *NOT* thread-safe
* assume standard operations are *NOT* atomic

Useful Gems:

* [thread_safe](https://github.com/headius/thread_safe)
* [atomic](https://github.com/headius/ruby-atomic)


Generally, see the above 4 rules and **be cognizant** of what you are doing

#### Actually Making Threads

<table>
  <tr>
		<th></th>
		<th> Creation </th>
		<th> Using </th>
		<th> + </th>
		<th> - </th>
		<th> Gotchas / Nuances </th>
	</tr>
	<tr>
		<th> Native </th>
		<td> -- </td>
		<td> `Thread.new {}` </td>
		<td> easy </td>
		<td> fat </td>
		<td> limit of ~10,000 threads </td>
	</tr>
	<tr>
		<th> Fixed Thread Pool </th>
		<td> `executor = Executors.new_fixed_thread_pool(num)` </td>
		<td> `executor.submit {}` </td>
		<td> persistent </td>
		<td> user-enforced quantity limit </td>
		<td> may block while waiting for available threads </td>
	</tr>
	<tr>
		<th> Cached Thread Pool </th>
		<td> `executor = Executors.new_cached_thread_pool` </td>
		<td> `executor.submit {}` </td>
		<td> no quantity limit </td>
		<td> idle threads reaped </td>
		<td> default idle time is 60 seconds </td>
	</tr>
</table>

#### Additional Resources

* [JRuby concurrency doc](https://github.com/jruby/jruby/wiki/Concurrency-in-jruby)
* [Engine Yard doc](https://blog.engineyard.com/2011/concurrency-in-jruby)
