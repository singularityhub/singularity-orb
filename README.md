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
