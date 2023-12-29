# openjdk-docker ![Build](https://github.com/team-carepay/openjdk-docker/workflows/publish/badge.svg)
OpenJDK Docker images with build tools. These images are optimized for size, and contain several tools that are useful in CI/CD pipelines.

| OS \ JDK           | 8                                                                                                       | 11                                                                                                        | 17                                                                                                         | 21                                                                                                        |
|--------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Debian Buster Slim | [8-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=8-slim)     | [11-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=11-slim)     | [17-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=17-slim)      | [21-slim](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=21-slim)     |
| Alpine             | [8-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=8-alpine) | [11-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=11-alpine) | [17-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=17-alpine)  | [21-alpine](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=21-alpine) |
| Zulu               | [8-zulu](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=8-zulu)     | [11-zulu](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=11-zulu)     | [17-zulu](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=17-zulu)      | [21-zulu](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=21-zulu)     | 

The following packages are included:
* curl
* git
* jq
* OpenJDK
* OpenSSH client
* unzip
* yq

[nojdk](https://hub.docker.com/r/carepaydev/openjdk/tags?page=1&ordering=last_updated&name=nojdk-nojdk)
### License: Apache 2.0
