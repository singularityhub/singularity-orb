# Singularity Orb

This is the [Circle CI Orb](https://circleci.com/orbs/registry/) to help you 
interact with [Singularity containers](https://www.github.com/sylabs/singularity).
Please see the [published orb](https://circleci.com/orbs/registry/orb/singularity/singularity)
to get the latest version, or look into the
[VERSION](VERSION) file here. The documentation will state 1.0.0 but we are beyond
that version.

## Versions

 - 1.0.3: coincides with Singularity 3.1.0 and 2.6.1 as default. There is a bug with the singularity-version variable so it doesn't change from 3.1.0.
 - 1.0.4: bug above is fixed, and default Singularity for 3.x is 3.2.1.
 - 1.0.5: **do not use** cache added for Singularity (debian bases, not Docker) based on Singularity version, however while the Orb tested to work, in production it breaks Singularity to install to the user home. This version should not be used.
 - 1.0.6: a re-release of 1.0.4, since CircleCI doesn't allow taking versions down unless there is a security reason.
 - 1.0.7: a redo with cache, this time working to correctly change permissions.

## Development

Before this Orb was used / tested with CircleCI, I needed to [install](https://circleci.com/docs/2.0/creating-orbs/)
the CircleCI client, and then validate it locally:

```bash
$ circleci orb validate src/orb.yml
```

and also deployed a development alpha version


```bash
$ circleci orb publish src/orb.yml singularity/singularity@dev:alpha
```

## Examples

The examples below describe the `.circleci/config.yml` file in your repository!
Generally, the examples below will vary on:

 - The executor (either a machine or Docker base image)
 - The versions of software installed (e.g., Singularity or GoLang)
 
@vsoch has tried to provide you with all the combinations that you might need to
build and interact with containers. For example, building a container using a
pre-built Docker base is the fastest, but you couldn't interact with it.
Using a machine base (and installing GoLang and Singularity, maximally) would
give you more functionality, but the build steps would take longer (and caching is
recommended).

### Build Examples

#### Fast Build

For the fastest build, you can use a pre-built Docker container. Since the
dependencies are inside the container, you can specify any version of Singularity.
The version coincides with a Docker tag of [singularityware/singularity](https://hub.docker.com/r/singularityware/singularity/tags)
on Docker Hub.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  build_container_docker_base_example:
    jobs:
      - singularity/build_container_docker_base:
          from-uri: docker://busybox
          image: busybox.sif
```



#### Debian Base with Custom (Singularity 2) Version

It's more likely that you'd want a machine (debian) base with Singularity if you
want to otherwise interact with your container. Here we install Singularity
from source.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  build_container_custom_2_example:
    jobs:
      - singularity/build_container_custom_2:
          singularity-version: 2.6.1
          from-uri: docker://busybox
          image: busybox.sif
```


#### Debian Base with Custom (Singularity 3) Version

And the same for the Singularity 3.* family. Notice you also have the
choice to define the version of GoLang, since we need to install it (the
default is 11.1.5). This option will give you the most freedom to use the 
container after building.


```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  build_container_custom_3_example:
  jobs:
    - singularity/build_container_custom_3:
        go-version: 1.11.5
        singularity-version: 3.2.1
        from-uri: docker://busybox
        image: busybox.sif
```


#### Docker Base with Custom (Singularity 3) Version

You can also install a custom version of Singularity into an alpine base image
that already has GoLang installed.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  build_container_docker_custom_3_example:
  jobs:
    - singularity/build_container_docker_custom_3:
        singularity-version: 3.2.1
        go-version: 1.11.5
        from-uri: docker://busybox
        image: busybox.sif
```


### Install

#### Install Debian, Singularity 2.*

This is more likely to be useful to you if you want to otherwise interact
with your container, beyond build. Here is how to build a 2.* derivative of
Singularity. We aren't using Docker.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  install_debian_2_example:
    jobs:
      - singularity/install_debian_2:
        singularity-version: 2.6.1
```

#### Install Debian, Singularity 3.*

Let's say that you want the same, but you want to install a specific
version of Singularity (over 3.0). Here we can use an alpine Docker base
(comes with GoLang) to add Singularity on top of it. 

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  install_debian_3_example:
    jobs:
      - singularity/install_debian_3:
        go-version: 11.1.5
        singularity-version: 3.2.1
```

#### Install Docker, Singularity 3.*

You can also customize the version of Singularity installed in an alpine
Docker base.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

  workflows:
    install_alpine_docker_3_example:
      jobs:
        - singularity/install_alpine_docker_3:
            singularity-version: 3.2.1
```


### Other

#### Interaction with Singularity via Docker Client

To just get the Docker base image with Singularity (no build) you can do that:

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

  workflows:
    docker_cli_example:
      jobs:
        - singularity/docker_cli:
            singularity-version: 3.2.1-slim
```

I don't have any good examples of using the above yet, please contribute yours if you do!
A repository with a starter example is available at 
[singularityhub/singularity-orb-example](https://github.com/singularityhub/singularity-orb-example).


### In the Wild

Here are some examples of Orbs in the wild.

 - [Singularity Python Client](https://github.com/singularityhub/singularity-cli/blob/master/.circleci/config.yml): uses Orb commands to install different versions of Singularity.

### What if I need help?

Please [open an issue](https://www.github.com/singularityhub/singularity-orb)!
@vsoch is relatively new to Orbs, and will help you to write a configuration,
or if a functionality is missing, write a new singularity orb. Right now
we just have build because that's the most common continuous integration need.
