# openjdk-docker ![Build](https://github.com/team-carepay/openjdk-docker/workflows/publish/badge.svg)

OpenJDK Docker images with build tools. These images are optimized for size, and contain several tools that are useful in CI/CD pipelines.

| OS \ JDK           | 11                                                                                                        | 17                                                                                                        | 21                                                                                                        | nojdk                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Debian Buster Slim | [11-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=11-slim)     | [17-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=17-slim)     | [21-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=21-slim)     | [nojdk-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=nojdk-slim)     |
| Alpine             | [11-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=11-alpine) | [17-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=17-alpine) | [21-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=21-alpine) | [nojdk-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=nojdk-alpine) |

The following packages are included:

- AWS CLI (optional)
- curl
- docker cli (include AWS ECR credentials helper)
- docker compose
- git
- gpg
- jq
- OpenJDK
- OpenSSH client
- unzip
- yq

Please note that we've also included a variant _without_ OpenJDK, this may be used for scripts that require the above tools, but do not need a JDK.

### License: Apache 2.0
