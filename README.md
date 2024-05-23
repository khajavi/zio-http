[//]: # (This file was autogenerated using `zio-sbt-website` plugin via `sbt generateReadme` command.)
[//]: # (So please do not edit it manually. Instead, change "docs/index.md" file or sbt setting keys)
[//]: # (e.g. "readmeDocumentation" and "readmeSupport".)

# ZIO Http

ZIO HTTP is a scala library for building http apps. It is powered by ZIO and [Netty](https://netty.io/) and aims at being the defacto solution for writing, highly scalable and performant web applications using idiomatic Scala.

[![Development](https://img.shields.io/badge/Project%20Stage-Development-green.svg)](https://github.com/zio/zio/wiki/Project-Stages) ![CI Badge](https://github.com/zio/zio-http/workflows/Continuous%20Integration/badge.svg) [![Sonatype Releases](https://img.shields.io/nexus/r/https/oss.sonatype.org/dev.zio/zio-http_2.13.svg?label=Sonatype%20Release)](https://oss.sonatype.org/content/repositories/releases/dev/zio/zio-http_2.13/) [![Sonatype Snapshots](https://img.shields.io/nexus/s/https/oss.sonatype.org/dev.zio/zio-http_2.13.svg?label=Sonatype%20Snapshot)](https://oss.sonatype.org/content/repositories/snapshots/dev/zio/zio-http_2.13/) [![javadoc](https://javadoc.io/badge2/dev.zio/zio-http-docs_2.13/javadoc.svg)](https://javadoc.io/doc/dev.zio/zio-http-docs_2.13) [![ZIO Http](https://img.shields.io/github/stars/zio/zio-http?style=social)](https://github.com/zio/zio-http)

## Installation

Setup via `build.sbt`:

```scala
libraryDependencies += "dev.zio" %% "zio-http" % "3.0.0-RC7"
```

**NOTES ON VERSIONING:**

- Older library versions `1.x` or `2.x` with organization `io.d11` of ZIO Http are derived from Dream11, the organization that donated ZIO Http to the ZIO organization in 2022.
- Newer library versions, starting in 2023 and resulting from the [ZIO organization](https://dev.zio) started with `0.0.x`, reaching `1.0.0` release candidates in April of 2023

## Getting Started

ZIO HTTP provides a simple and expressive API for building HTTP applications. It supports both server and client-side APIs. 

ZIO HTTP is designed in terms of **HTTP as function**, where both server and client are a function from `Request` to `Response`.

### Greeting Server

The following example demonstrates how to build a simple greeting server. It contains 2 routes: one on the root
path, it responds with a fixed string, and one route on the path `/greet` that responds with a greeting message
based on the query parameter `name`.

```scala
import zio._
import zio.http._

object GreetingServer extends ZIOAppDefault {
  val routes =
    Routes(
      Method.GET / Root -> handler(Response.text("Greetings at your service")),
      Method.GET / "greet" -> handler { (req: Request) =>
        val name = req.queryParamToOrElse("name", "World")
        Response.text(s"Hello $name!")
      }
    )

  def run = Server.serve(routes).provide(Server.default)
}
```

### Greeting Client

The following example demonstrates how to call the greeting server using the ZIO HTTP client:

```scala
import zio._
import zio.http._

object GreetingClient extends ZIOAppDefault {

  val app =
    for {
      client   <- ZIO.serviceWith[Client](_.host("localhost").port(8080))
      request  =  Request.get("greet").addQueryParam("name", "John")
      response <- client.request(request)
      _        <- response.body.asString.debug("Response")
    } yield ()

  def run = app.provide(Client.default, Scope.default)
}
```

## Documentation

Learn more on the [ZIO Http homepage](https://github.com/zio/zio-http)!

## Contributing

For the general guidelines, see ZIO [contributor's guide](https://zio.dev/contributor-guidelines).

## Code of Conduct

See the [Code of Conduct](https://zio.dev/code-of-conduct)

## Support

Come chat with us on [![Badge-Discord]][Link-Discord].

[Badge-Discord]: https://img.shields.io/discord/629491597070827530?logo=discord "chat on discord"
[Link-Discord]: https://discord.gg/2ccFBr4 "Discord"

## License

[License](LICENSE)
