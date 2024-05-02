# `Barrier`

A group of tasks cooperate by moving through a series of phases, where each in the group signals it has arrived at the `Barrier` in a given phase and implicitly waits for all others to arrive.

The following example shows how to use a barrier:

```csharp
BarrierSample();

void BarrierSample()
{
    int count = 0;

    // Create a barrier with 3 participants.
    // Provide a post-phase action that will print out certain information.
    // And the third time through, it will throw an exception.
    var barrier = new Barrier(3, b =>
    {
        Console.WriteLine($"Post-Phase action: count={count}, phase={b.CurrentPhaseNumber}");
        
        if (b.CurrentPhaseNumber == 2) 
            throw new Exception("D'oh!");
    });

    // Nope — changed my mind.  Let's make it five participants.
    barrier.AddParticipants(2);

    // Nope — let's settle on four participants.
    barrier.RemoveParticipant();

    // This is the logic run by all participants.
    Action action = () =>
    {
        Interlocked.Increment(ref count);
        barrier.SignalAndWait(); // During the post-phase action, count should be 4 and phase should be 0.
        
        Interlocked.Increment(ref count);
        barrier.SignalAndWait(); // During the post-phase action, count should be 8 and phase should be 1.

        // The third time, SignalAndWait() will throw an exception and all participants will see it.
        Interlocked.Increment(ref count);
        try
        {
            barrier.SignalAndWait();
        }
        catch (BarrierPostPhaseException bppe)
        {
            Console.WriteLine("Caught BarrierPostPhaseException: {0}", bppe.Message);
        }

        // The fourth time should be hunky-dory.
        Interlocked.Increment(ref count);
        barrier.SignalAndWait(); // During the post-phase action, count should be 16 and phase should be 3.
    };

    // Now launch 4 parallel actions to serve as 4 participants.
    Parallel.Invoke(action, action, action, action);

    // This (5 participants) would cause an exception:
    // Parallel.Invoke(action, action, action, action, action);
    //      "System.InvalidOperationException: The number of threads using the barrier
    //      exceeded the total number of registered participants."

    // It's good form to Dispose() a barrier when you're done with it.
    barrier.Dispose();
}
```

## Thread safety

All public and protected members of `Barrier` are thread-safe and may be used concurrently from multiple threads, with the exception of `Dispose`, which must only be used when all other operations on the `Barrier` have completed.
