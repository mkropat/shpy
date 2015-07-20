# shpy

*Spies and stubs for shell unit testing*

Features at a glance:

* Create spies for any command or function in the shell environment
* Stub stdout and return value of spies
* See call count and check arguments passed to the spy
* [Integrates](#shunit2-integration) with the [shunit2](https://github.com/kward/shunit2) testing framework

## API

To use **shpy** in your tests, source the `shpy` script:

	. path/to/shpy
	
A summary of functions:

Function                                 | Description
-----------------------------------------|-------------------------------------------------------------------------------
<pre>createSpy SPY</pre>                 | create a new spy named SPY, or reset SPY if it already existed
<pre>createSpy -r RETURN_VAL SPY</pre>   | `createSpy` accepts an option `-r`, the value that SPY returns
<pre>createSpy -o OUTPUT SPY</pre>       | `createSpy` accepts an option `-o`, the text that SPY outputs when run
<pre>createStub STUB</pre>               | alias for `createSpy`
<pre>getSpyCallCount SPY</pre>           | output the number of times a spy has been called
<pre>wasSpyCalledWith SPY [ARG]...</pre> | test if a spy was called with the given arguments
<pre>examineNextSpyCall SPY</pre>        | tell `wasSpyCalledWith` to test on the spy's next call<br/>(by default, `wasSpyCalledwith` tests the first call to a spy — after calling `examineNextSpyCall`, `wasSpyCalledWith` will test against the second call, and so on)
<pre>cleanupSpies</pre>                  | after you are done testing, call this function to clean up any temporary directories created

### shunit2 Integration

To use **shpy** asserts in your **shunit2** tests, you must also source the
`shpy-shunit2` script:

	. path/to/shpy
	. path/to/shpy-shunit2
	
A summary of asserts:

Function                                              | Description
------------------------------------------------------|------------------------------------------------------------------
<pre>assertCallCount [MESSAGE] SPY COUNT</pre>        | asset SPY was called COUNT times
<pre>assertCalledWith SPY [ARG]...</pre>              | assert SPY was called with ARGs; also calls `examineNextSpyCall` so that calling `assertCalledWith</pre> again asserts on the next call to SPY
<pre>assertCalledWith_ MESSAGE SPY [ARG]...</pre>     | same, but include MESSAGE in failure output
<pre>assertCalledOnceWith SPY [ARG]...</pre>          | convenience assert for `assertCallCount SPY 1 && assertCalledWith SPY ARGs`
<pre>assertCalledOnceWith_ MESSAGE SPY [ARG]...</pre> | same, but include MESSAGE in failure output
<pre>assertNeverCalled [MESSAGE] SPY</pre>            | convenience assert for `assertCallCount SPY 0`

Use the `oneTimeTearDown` hook provided by **shunit2** to clean up any spies:

    oneTimeTearDown() {
        cleanupSpies
    }

## A Word On Shell Portability

While **shunit2** remains strictly [POSIX
compliant](http://shellhaters.heroku.com/posix), **shpy** relies on [portable but more modern shell features](http://apenwarr.ca/log/?m=201102#28), such as
function-local variables.  To be clear, **shpy** does not use any
[Bashisms](https://wiki.ubuntu.com/DashAsBinSh).
