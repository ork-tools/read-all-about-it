---
layout: page
title: Processors
---
_Processors_ provide orchestrations in the system [[core]].

* They generally form the command API for the system core.
* They define the [[business transaction]] boundaries. 
* They often coordinate operations between multiple [[aggregates]].
* 

In the **ork** model, processors are considered part of the domain.

## PietroFi Implementation

[[PietroFi]] is an **ork** tool that transforms ordinary C# process code into robust [[state machine]]s that can be hosted by the [[Pietro Core]].

To create a processor, just create a C# class with a few requirements:

* must be `public partial` and derived directly from `PietroFi.Processor`
* must accept an `IMessageContext` in its constructor(s) and passes it the base class
* must define any fields as `readonly`

{% highlight C# %}
// simple example
public sealed partial class SimplestProcessor : Processor
{
   public SimplestProcessor(IMessageContext context) : base(context)
   {
   }
}

// more complex example
public abstract partial class ProcessorWithServices : Processor
{
   public ProcessorWithServices(IMessageContext context, IExternalService aService,
      IOtherExternalService anotherService)
      : base(context)
   {
      _aService = aService;
      _anotherService = anotherService;
   }

   protected readonly IExternalService _aService;
   protected readonly IOtherExternalService _anotherService;
}
{% endhighlight %}

Within the processor class, create process methods that take messages:

* must be `public async`
* must return `Task` or `Task<T>`
* must be named `Process`
* must take a single parameter that implements `IMessage`

{% highlight C# %}
// assumes Calculate implements IMessage
// assumes Calculate the answer is an async method available to the processor
public async Task<int> Process(Calculate command)
{
   return await CalculateTheAnswer(command.Values);
}

// assumes TellEveryone implements IMessage
public async Task Process(TellEveryone command)
{
   await Task.WhenAll(_aService.Tell(command.Message), _anotherService.Tell(command.Message));
}
{% endhighlight %}
