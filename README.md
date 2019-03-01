# Singularity Orb

This is the [Circle CI Orb](https://circleci.com/orbs/registry/) to help you 
interact with [Singularity containers](https://www.github.com/sylabs/singularity).
More notes to come!

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
Here is a very basic example to build a container - this will use the
default Singularity version 3.1. See the [VERSION](VERSION) file to
get the current published version.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.1

workflows:
  build_example:
    jobs:
      - singularity/build_container:
          from-uri: docker://busybox 
          image: busybox.sif 
```

You can also specify a custom version of Singularity (this corresponds to
the version that you would clone from GitHub - the builder will use a go
base docker image and install Singularity to it:

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.1

workflows:
  build_example:
    jobs:
      - singularity/build_container:
          singularity-version: 3.0.2
          from-uri: docker://busybox 
          image: busybox.sif 
```

But if you want to just use a Docker container from [singularityware/singularity](https://hub.docker.com/r/singularityware/singularity/tags)
the job name that you want is `singularity/build_container_docker` and the default `singularity-version` corresponds to `3.1-slim`.

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.1

workflows:
  build_example:
    jobs:
      - singularity/build_container_docker:
          from-uri: docker://busybox 
          image: busybox.sif 
```

*This is the fastest way to build!*

Finally, if you want an older version of Singularity (pre GoLang) you want to do this:

```yaml
version: 2.1

orbs:
  singularity: singularity/singularity@1.0.1

workflows:
  build_example:
    jobs:
      - singularity/build_container_version_2:
          from-uri: docker://busybox
          image: busybox.sif 
```

The default `singularity-version` is 2.6.1, again corresponding to the [Github tag](https://github.com/sylabs/singularity/releases).

A repository with a starter example is available at 
[singularityhub/singularity-orb-example](https://github.com/singularityhub/singularity-orb-example).

### What if I need help?

Please [open an issue](https://www.github.com/singularityhub/singularity-orb)!
@vsoch is relatively new to Orbs, and will help you to write a configuration,
or if a functionality is missing, write a new singularity orb. Right now
we just have build because that's the most common continuous integration need.
