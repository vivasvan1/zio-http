# Configuration Documentation

import zio.config.\_
import zio.config.magnolia.DeriveConfigDescriptor
import java.nio.file.{Files, Paths}

object ConfigurationDocs {
def generateConfigurationDocs(): String = {
val config: Config[ConnectionPoolConfig] = {
val disabled = Config.string.mapOrFail {
case "disabled" => Right(ConnectionPoolConfig.Disabled)
case other => Left(Config.Error.InvalidData(message = s"Invalid value for ConnectionPoolConfig: $other"))
}
val fixed = Config.int("fixed").map(ConnectionPoolConfig.Fixed.apply)
val dynamic = (Config.int("minimum") ++ Config.int("maximum") ++ Config.duration("ttl"))
.nested("dynamic")
.map { case (minimum, maximum, ttl) =>
ConnectionPoolConfig.Dynamic(minimum, maximum, ttl)
}
val fixedPerHost = (Config
.chunkOf(
"per-host",
(Config.string("url") ++ fixed).mapOrFail { case (s, fixed) =>
URL
.decode(s)
.left
.map(error => Config.Error.InvalidData(message = s"Invalid URL: ${error.getMessage}"))
.flatMap { url =>
url.kind match {
case url: URL.Location.Absolute => Right(url -> fixed)
case _ => Left(Config.Error.InvalidData(message = s"Invalid value for ConnectionPoolConfig: $s"))
}
}
},
) ++ fixed.nested("default")).nested("fixed")
.map { case (perHost, default) =>
ConnectionPoolConfig.FixedPerHost(perHost.toMap, default)
}
val dynamicPerHost = (Config
.chunkOf(
"per-host",
(Config.string("url") ++ dynamic).mapOrFail { case (s, fixed) =>
URL
.decode(s)
.left
.map(error => Config.Error.InvalidData(message = s"Invalid URL: ${error.getMessage}"))
.flatMap { url =>
url.kind match {
case url: URL.Location.Absolute => Right(url -> fixed)
case _ => Left(Config.Error.InvalidData(message = s"Invalid value for ConnectionPoolConfig: $s"))
}
}
},
) ++ dynamic.nested("default")).nested("dynamic")
.map { case (perHost, default) =>
ConnectionPoolConfig.DynamicPerHost(perHost.toMap, default)
}

      disabled orElse fixed orElse dynamic orElse fixedPerHost orElse dynamicPerHost
    }

    val docs: Docs = generateDocs(config)
    val markdown: String = docs.toTable.toGithubFlavouredMarkdown

    markdown

}

def writeConfigurationDocsToFile(): Unit = {
val markdown = generateConfigurationDocs()
val filePath = "docs/Configuration.md"

    Files.write(Paths.get(filePath), markdown.getBytes)

}

def main(args: Array[String]): Unit = {
writeConfigurationDocsToFile()
}
}
