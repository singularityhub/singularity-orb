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

Here is a very basic example to build a container - this is the `.circleci/config.yml`
file in your repository!

```yaml
# CircleCI build config to test the Google Cloud Platform Container Registry Orb published by CircleCI

version: 2.1

orbs:
  singularity: singularity/singularity@1.0.0

workflows:
  build_example:
    jobs:
      - singularity/build_container:
          from-uri: docker://busybox 
          image: busybox.sif 
          filters:
            branches:
              only: master
```

A repository with this example is available at 
[singularityhub/singularity-orb-example](https://github.com/singularityhub/singularity-orb-example).

### What if I need help?

Please [open an issue](https://www.github.com/singularityhub/singularity-orb)!
@vsoch is relatively new to Orbs, and will help you to write a configuration,
or if a functionality is missing, write a new singularity orb. Right now
we just have build because that's the most common continuous integration need.
