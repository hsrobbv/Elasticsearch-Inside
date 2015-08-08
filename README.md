![](https://raw.githubusercontent.com/poulfoged/elasticsearch-inside/master/logo.png) &nbsp; ![](https://ci.appveyor.com/api/projects/status/prwp3j290469ntpb/branch/master?svg=true) &nbsp; ![](http://img.shields.io/nuget/v/elasticsearch-inside.svg?style=flat)

A fully embedded version of [Elasticsearch][Elasticsearch] for integration tests. When the instance is created both the jvm and elasticsearch itself is extracted to a temporary location (2-3 seconds in my tests) and started (5-6 seconds in my tests). Once disposed everything is removed again.

The instance will be started on a random port - full url available as Url property.

To use elasticsearch in integration tests simply instanciate it - in these tests I'm using the excellent client [Elasticsearch-NEST][nest]:
using (var elasticsearch = new Elasticsearch())
{
    ////Arrange
    var client = new ElasticClient(new ConnectionSettings(elasticsearch.Url));

    ////Act
    var result = client.Ping();

    ////Assert
    Assert.That(result.IsValid);
}

```c#
using (var elasticsearch = new Elasticsearch(c => c
    .Port(444)
    .AddArgument("-Des.script.engine.groovy.file.aggs=on")))
{
    ////Arrange
    var client = new ElasticClient(new ConnectionSettings(elasticsearch.Url));

    ////Act
    var result = client.Ping();

    ////Assert
    Assert.That(result.IsValid);
    Assert.That(elasticsearch.Url.Port, Is.EqualTo(444));
}

```c#
using (new Elasticsearch(c => c.EnableLogging().LogTo(Console.WriteLine)))
{
                
}

Simply add the Nuget package:

`PM> Install-Package elasticsearch-inside`

## Requirements

You'll need .NET Framework 4.5.1 or later to use the precompiled binaries.

## License

Elasticsearch Inside is under the MIT license. 

[nest]: https://github.com/elastic/elasticsearch-net  "Elasticsearch.Net & NEST"

