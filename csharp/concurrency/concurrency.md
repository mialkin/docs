# Concurrency

**Concurrency** is doing more than one thing at a time.<sup>1</sup>

There are several kinds of concurrency:

* [Asynchronous programming](asynchronous%20programming.md)
* [Parallel programming](parallel%20programming.md)
* [Reactive programming](reactive%20programming.md)
* [Dataflow programming](dataflow%20programming.md)

Usually, a mixture of techniques is used when writing a concurrent program. Most applications at least use multithreading (via the thread pool) and asynchronous programming. Feel free to mix and match all the various forms of concurrency, using the appropriate tool for each part of the application.

## Concurrency vs parallelism

*Concurrency* and *parallelism* are not the same thing. **Concurrency** is a way to build things — it's a composition of independently executing things, called *procesees*, typically functions, but they don't have to be. **Parallelism** is, on the other hand, is *simultaneous* execution of multiple things, possibly related, possibly not.

If you think about in general way, then concurrency is about *dealing* with a lot of things at once, and parallelism is about *doing* a lot of things at once — those are related, but actually separate ideas. Concurrency is about *structure*, parallelism is about *execution*. <br><br>Concurrency is about structuring things, so that you can, *maybe*, use parallelism to do a better job, but parallelism is not the goal of concurrency; concurrency's goal is a good structure.

In order to make concurrency work, you have to add this idea of *communication*. Concurrency gives you a way to structure program into independent pieces, but then you have to coordinate those pieces. And to make that work you need some form of communication. And Tony Hoare in 1978 wrote a paper called "Communicating sequential processes" which is trully one of the greatest papers in computer science.<sup>2</sup>

<hr>

<sup>1</sup> Stephen Cleary, Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming, Second edition (O'Reilly Media, 2019).

<sup>2</sup> Rob Pike, Concurrency Is Not Parallelism, [talk at Heroku conference ↑](https://vimeo.com/49718712), 2003.
