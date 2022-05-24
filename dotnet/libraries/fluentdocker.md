# FluentDocker

[↑ FluentDocker](https://github.com/mariotoffia/FluentDocker) is a .NET library that enables `docker` and `docker-compose` interactions using Fluent API.

## Installation

```bash
dotnet add package Ductus.FluentDocker
```

## Postgres example

```csharp
using Ductus.FluentDocker.Builders;
using Ductus.FluentDocker.Extensions;
using Ductus.FluentDocker.Services;
using Ductus.FluentDocker.Services.Extensions;
using FluentAssertions;

var builder = new Builder();

var containerService = builder
    .UseContainer()
    .UseImage("postgres:12")
    .ExposePort(5432)
    .WithEnvironment("POSTGRES_PASSWORD=mysecretpassword")
    .WaitForPort("5432/tcp", millisTimeout: 30000 /*30s*/)
    .Build()
    .Start();

int hostPort = containerService.ToHostExposedEndpoint(portAndProto: $"{5432}/tcp").Port;

var container = containerService.GetConfiguration(fresh: true);

container.State.ToServiceState().Should().Be(ServiceRunningState.Running);

containerService.Stop();
containerService.Remove();
```

## Kafka example using docker-compose file

```csharp
using Ductus.FluentDocker.Builders;
using Ductus.FluentDocker.Model.Common;
using Ductus.FluentDocker.Services.Extensions;
using FluentAssertions;

var file = Path.Combine(Directory.GetCurrentDirectory(),
    (TemplateString) "/Users/j.doe/Downloads/docker-compose.yml");

using var compositeService = new Builder()
    .UseContainer()
    .UseCompose()
    .FromFile(file)
    .RemoveOrphans()
    .WaitForPort(service:"kafka",portAndProto:"9092/tcp", millisTimeout: 30000 /*30s*/) 
    .Build().Start();

compositeService.Hosts.Count.Should().Be(1); // The host used by compose
compositeService.Containers.Count.Should().Be(3); // We can access each individual container
compositeService.Images.Count.Should().Be(3); // And the images used.

var containerService = compositeService.Containers.First(x => x.Name == "kafka");

int port = containerService.ToHostExposedEndpoint(portAndProto: "9092/tcp").Port;
```

## Kafka example using Fluent API

```csharp
using Ductus.FluentDocker.Builders;
using Ductus.FluentDocker.Extensions;
using Ductus.FluentDocker.Services;
using Ductus.FluentDocker.Services.Extensions;
using FluentAssertions;

IContainerService GetZookeeperContainerService()
{
    var builder = new Builder();
    
    var containerService = builder
        .UseContainer()
        .WithName("zookeeper-2")
        .UseNetwork("my_kafka")
        .UseImage("wurstmeister/zookeeper")
        .ExposePort(2181)
        .WithEnvironment("ZOOKEEPER_CLIENT_PORT=2181") //Instructs ZooKeeper where to listen for connections by clients such as Apache Kafka®.
        .WaitForPort("2181/tcp", millisTimeout: 30000 /*30s*/)
        .Build()
        .Start();

    return containerService;
}

IContainerService GetKafkaContainerService()
{
    var builder = new Builder();
    
    var environmentVariables = new Dictionary<string, string>();

    environmentVariables.Add("KAFKA_BROKER_ID", "1");
    environmentVariables.Add("KAFKA_ZOOKEEPER_CONNECT", "zookeeper-2:2181");
    environmentVariables.Add("KAFKA_LISTENERS", "CLIENT://kafka-2:9092,EXTERNAL://kafka-2:9093");
    environmentVariables.Add("KAFKA_ADVERTISED_LISTENERS", "CLIENT://kafka-2:9092,EXTERNAL://127.0.0.1:9093");
    environmentVariables.Add("KAFKA_LISTENER_SECURITY_PROTOCOL_MAP", "CLIENT:PLAINTEXT,EXTERNAL:PLAINTEXT");
    environmentVariables.Add("KAFKA_INTER_BROKER_LISTENER_NAME", "CLIENT");
    environmentVariables.Add("KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR", "1");

    var topics = new List<string>
    {
        "your.topic1.name:1:1",
        "your.topic2.name:1:1",
    };
    
    environmentVariables.Add("KAFKA_CREATE_TOPICS", string.Join(",", topics));

    var keyValueString = environmentVariables.Select(x => $"{x.Key}={x.Value}").ToArray();

    var containerService = builder
        .UseContainer()
        .UseNetwork("my_kafka")
        .UseImage("wurstmeister/kafka")
        .WithName("kafka-2")
        .ExposePort(9092)
        .ExposePort(9093)
        .WithEnvironment(keyValueString)
        .WaitForPort("9092/tcp", millisTimeout: 30000 /*30s*/)
        .Build()
        .Start();

    return containerService;
}

IContainerService GetKafkaUiContainerService()
{
    var builder = new Builder();
    
    var environmentVariables = new Dictionary<string, string>();

    environmentVariables.Add("KAFKA_CLUSTERS_0_NAME", "local");
    environmentVariables.Add("KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS", "kafka-2:9092");
    environmentVariables.Add("KAFKA_CLUSTERS_0_ZOOKEEPER", "zookeeper-2:2181");

    var keyValueString = environmentVariables.Select(x => $"{x.Key}={x.Value}").ToArray();

    var containerService = builder
        .UseContainer()
        .UseNetwork("my_kafka")
        .UseImage("provectuslabs/kafka-ui")
        .WithName("kafka-ui-2")
        .ExposePort(8080)
        .WithEnvironment(keyValueString)
        .Build()
        .Start();

    return containerService;
}

var zookeeperContainerService = GetZookeeperContainerService();
int zookeeperHostPort = zookeeperContainerService.ToHostExposedEndpoint(portAndProto: $"{2181}/tcp").Port;
var zookeeperContainer = zookeeperContainerService.GetConfiguration(fresh: true);
zookeeperContainer.State.ToServiceState().Should().Be(ServiceRunningState.Running);


var kafkaContainerService = GetKafkaContainerService();
int kafkaHostPort = kafkaContainerService.ToHostExposedEndpoint(portAndProto: $"{9092}/tcp").Port;
var kafkaContainer = kafkaContainerService.GetConfiguration(fresh: true);
kafkaContainer.State.ToServiceState().Should().Be(ServiceRunningState.Running);

var kafkaUiContainerService = GetKafkaUiContainerService();

zookeeperContainerService.Stop();
zookeeperContainerService.Remove();
kafkaContainerService.Stop();
kafkaContainerService.Remove();
kafkaUiContainerService.Stop();
kafkaUiContainerService.Remove();
```
