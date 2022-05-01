# Benchmarking

**Benchmarking** is a process of running a test that measures the performance of a software.

## BenchmarkDotNet

[↑ BenchmarkDotNet](https://github.com/dotnet/BenchmarkDotNet) is an open-source .NET library that helps to transform methods into benchmarks, track their performance, and share reproducible measurement experiments.

Under the hood, it performs a lot of magic that guarantees reliable and precise results thanks to the [↑ perfolizer](https://github.com/AndreyAkinshin/perfolizer) statistical engine. BenchmarkDotNet protects you from popular benchmarking mistakes and warns you if something is wrong with your benchmark design or obtained measurements. The results are presented in a user-friendly form that highlights all the important facts about your experiment.
