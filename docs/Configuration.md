# Configuration

This document provides an overview of the configuration options available in the ZIO HTTP library.

## ConnectionPoolConfig

```scala
import zio.config.magnolia.DeriveConfigDescriptor._
import zio.config.{ConfigDescriptor, generateDocs}

val descriptor: ConfigDescriptor[ConnectionPoolConfig] = descriptor[ConnectionPoolConfig]
val docs = generateDocs(descriptor).toTable.toGithubFlavouredMarkdown
println(docs)
```

## ZClient

```scala
import zio.config.magnolia.DeriveConfigDescriptor._
import zio.config.{ConfigDescriptor, generateDocs}

val descriptor: ConfigDescriptor[ZClient[_, _, _, _]] = descriptor[ZClient[_, _, _, _]]
val docs = generateDocs(descriptor).toTable.toGithubFlavouredMarkdown
println(docs)
```

## NettyConnectionPool

```scala
import zio.config.magnolia.DeriveConfigDescriptor._
import zio.config.{ConfigDescriptor, generateDocs}

val descriptor: ConfigDescriptor[NettyConnectionPool] = descriptor[NettyConnectionPool]
val docs = generateDocs(descriptor).toTable.toGithubFlavouredMarkdown
println(docs)
