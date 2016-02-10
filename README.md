Metrics Aggregator Protocol
===========================

<a href="https://raw.githubusercontent.com/ArpNetworking/metrics-aggregator-protocol/master/LICENSE">
    <img src="https://img.shields.io/hexpm/l/plug.svg"
         alt="License: Apache 2">
</a>
<a href="https://travis-ci.org/ArpNetworking/metrics-aggregator-protocol/">
    <img src="https://travis-ci.org/ArpNetworking/metrics-aggregator-protocol.png"
         alt="Travis Build">
</a>
<a href="http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.arpnetworking.metrics%22%20a%3A%22metrics-aggregator-protocol%22">
    <img src="https://img.shields.io/maven-central/v/com.arpnetworking.metrics/metrics-aggregator-protocol.svg"
         alt="Maven Artifact">
</a>

Defines the protocol between [Metrics Aggregator Daemon](https://github.com/ArpNetworking/metrics-aggregator-daemon) and [Metrics Cluster Aggregator](https://github.com/ArpNetworking/metrics-cluster-aggregator).

Framing
-------
Since protobuf does not have a message framing system, a prefix message length and message type has been added to the protocol.

Each message in the stream is prefixed with a 32-bit, big endian size (in bytes) of the message (including the size header). After the size comes a variable length message type.  Currently, only 1 and 2 byte types are used.  The message types are listed below.

Code | Type
-----|---------------------
0x01 | HostIdentification
0x03 | Heartbeat
0x04 | StatisticsSet
0x05 | SampleSupportingData

Only SampleSupportingData currently has a subtype, which describes how to deserialize the supporting data.

Type and Subtype Code | SubType
----------------------|---------------------------------
0x05 0x01             | Samples supporting data
0x05 0x02             | Sparse histogram supporting data

The full message format is:

     | 4 byte header | 1 byte type | [ 1 byte subtype (optional) ] | n byte protobuf payload


Building
--------

Prerequisites:
* [JDK8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Building:

    metrics-aggregator-protocol> ./mvnw verify

To use the local version you must first install it locally:

    metrics-aggregator-protocol> ./mvnw install

You can determine the version of the local build from the pom file.  Using the local version is intended only for testing or development.

You may also need to add the local repository to your build in order to pick-up the local version:

* Maven - Included by default.
* Gradle - Add *mavenLocal()* to *build.gradle* in the *repositories* block.
* SBT - Add *resolvers += Resolver.mavenLocal* into *project/plugins.sbt*.

License
-------

Published under Apache Software License 2.0, see LICENSE

&copy; Groupon Inc., 2016
