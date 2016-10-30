Name:	Async_Call
Scope:	Buffering a source process
Summary:	  Provide a method that calls that starts a process that calls the source process and returns a reference. The spawned process sends a message to the caller on completion.  The reference links the initial call with the returned data.

my_async_call(Parameters) ->
  Caller = self(),
  Ref = make_ref(),
  spawn(fun() ->
  	      Result = server:method(Parameters),
	      Caller ! {Ref,Result} end),
  Ref.

The async nature of the call allows the buffer to initiate calls and handle, data requests while the call is being evaluated.  Moving the async call out of the source server simplifies the design of the server in that it only need handle blocking calls.
