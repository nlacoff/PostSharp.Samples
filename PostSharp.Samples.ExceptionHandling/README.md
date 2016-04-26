# PostSharp.Samples.ExceptionHandling

Handling exceptions is bread and butter of every programmer, especially the one using the code throwing them. Surely it did happen to you 
that you had no idea when and what might the method you call throw. Or worse, what a method called by that method might throw that the author did not 
anticipate. 

Frankly, this happens in every application and gets worse as its lifecycle progresses. As the time flows, you are pushed to patch the code here 
and there, for this kind of exception and that. Mostly, you handle exceptions that have nothing to do with the code where you need to handle them. You log 
them, ignore them, add at least some information that may help track their cause in production. You can see that it is plain wrong to do it everywhere
by changing the actual code, but you had no other options.

Now PostSharp changes your situation completely - you get aspects.

In this example you will first see how to use the `OnExceptionAspect` class to add contextual information to an exception using an aspect, without any need 
to change the actual code. Second, there will be an aspect for reporting and swallowing an exception. By combining these two aspects, you can achieve
better maintainability of your application without making the code unreadable and thus unmanageable.

The sample implements two aspects:

* `AddContextOnExceptionAttribute` is meant to be added to all methods in the assembly using the one-liner `[assembly:AddContextOnException]`. Its role is 
   to add the parameter values of the current value to the `Exception.Data["Context"]` object. Since the aspect is supposed to be applied to all 
   user-writen methods, the aspect will gather the parameter values of all user-writen methods in the call stack, and it will be a great help for 
   debugging. This aspect does not suppress the exception.
   
* `ReportAndSwallowExceptionAttribute` writes the exception to the console (including the data collected by  `AddContextOnExceptionAttribute`) and 
   suppresses the exception. Suppressing exceptions is dangerous and you cannot do that on each method. You would typically use this kind of aspects 
   on an event handler (such as WPF and WinForms ones) or a thread entrypoint. 

## Limitations

This example is not concerned with many details you are concerned with in your production code:

* Not thread-safe.
* Not optimized.
* Not having robust logging of exception.