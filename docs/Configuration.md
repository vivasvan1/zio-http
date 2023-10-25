# Configuration

This document provides an overview of the configuration options available in the ZIO HTTP library.

## ConnectionPoolConfig

```scala
import zio.http.ConnectionPoolConfig

println(ConnectionPoolConfig.generateConfigDocs)
```

## ZClient

```scala
import zio.config.magnolia.DeriveConfigDescriptor._
import zio.http.ZClient

println(ZClient.generateConfigDocs)
```

## NettyConnectionPool

```scala
import zio.config.magnolia.DeriveConfigDescriptor._
import zio.config.{ConfigDescriptor, generateDocs}

val descriptor: ConfigDescriptor[NettyConnectionPool] = descriptor[NettyConnectionPool]
val docs = generateDocs(descriptor).toTable.toGithubFlavouredMarkdown
println(docs)
```
