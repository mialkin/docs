# Dataflow programming

TPL Dataflow is capable of handling any kind of _mesh_. You can define forks, joins, and loops in a mesh, and TPL Dataflow will handle them appropriately. Most of the time, though, TPL Dataflow meshes are used as a pipeline.

The basic building unit of a dataflow mesh is a _dataflow block_. A block can either be a _target block_ (receiving data), a _source block_ (producing data), or both. Source blocks can be linked to target blocks to create the mesh. It's possible to break links and create new blocks and add them to the mesh _while_ there is data flowing through it, but that is a very advanced scenario.

Target blocks have buffers for the data they receive. Having buffers enables them to accept new data items even if they aren't ready to process them yet; this keeps data flowing through the mesh.

TPL Dataflow namespace:

```csharp
System.Threading.Tasks.Dataflow
```

## Dataflow vs System.Reactive

At first glance, dataflow meshes sound very much like observable streams, and they do have much in common. Both meshes and streams have the concept of data items passing through them. Also, both meshes and streams have the notion of a normal completion (a notification that no more data is coming), as well as a faulting completion (a notification that some error occurred during data processing). But System.Reactive (Rx) and TPL Dataflow do not have the same capabilities. Rx observables are generally better than dataflow blocks when doing anything related to timing. Dataflow blocks are generally better than Rx observables when doing parallel processing. Conceptually, Rx works more like setting up callbacks: each step in the observable directly calls the next step. In contrast, each block in a dataflow mesh is very independent from all the other blocks. Both Rx and TPL Dataflow have their own uses, with some amount of overlap. They also work quite well together.

## Dataflow vs actor frameworks

If you're familiar with actor frameworks, TPL Dataflow will seem to share similarities with them. Each dataflow block is independent, in the sense that it will spin up tasks to do work as needed, like executing a transformation delegate or pushing output to the next block. You can also set up each block to run in parallel, so that it'll spin up multiple tasks to deal with additional input. Due to this behavior, each block does have a certain similarity to an actor in an actor framework. However, TPL Dataflow is not a full actor framework; in particular, there's no built-in support for clean error recovery or retries of any kind. TPL Dataflow is a library with an actor-like feel, but it isn't a full-featured actor framework.
