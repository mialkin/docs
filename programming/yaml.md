# YAML

If you have repeated sections in your YAML file, you might like to use YAML anchors. They can reduce effort and make updating in bulk, easier.

## Table of contents

- [YAML](#yaml)
  - [Table of contents](#table-of-contents)
  - [Anchors and aliases](#anchors-and-aliases)
    - [Override values](#override-values)
  - [Links](#links)

## Anchors and aliases

There are 2 parts to this:

- The anchor `&` which defines a chunk of configuration
- The alias `*` used to refer to that chunk elsewhere

So in the example below we use `&build-test` to define a step entity, which has several lines for reuse, and the alias `*build-test` to reuse it.

```yml
definitions: 
  steps:
    - step: &build-test
        name: Build and test
        script:
          - mvn package
        artifacts:
          - target/**

pipelines:
  branches:
    develop:
      - step: *build-test
    main:
      - step: *build-test
```

> YAML anchors and aliases cannot contain the `[` , `]`, `{`, `}`, and `,` characters.

### Override values

But what if you want essentially the same block of code with one small change?

You can use overrides with the characters `<<:` to add more values, or override existing ones.

Building on the example above we could override the name value for a step:

```yml
pipelines:
  branches:
    develop:
      - step: *build-test
    main:
      - step: 
          <<: *build-test
          name: Testing on Main
```

In the pipelines result page, we'd see the name of the step as "Build and test" for pipelines that ran on the `develop` branch, and "Testing on Main" for pipelines that ran on the `main` branch.

## Links

- [↑ Three ways to reuse commands across Gitlab jobs](https://jsramblings.com/three-ways-to-reuse-commands-across-gitlab-jobs)
- [↑ YAML anchors](https://support.atlassian.com/bitbucket-cloud/docs/yaml-anchors)
- [↑ Optimize GitLab CI/CD configuration files](https://docs.gitlab.com/ee/ci/yaml/yaml_optimization.html)
- [↑ Learn YAML in Y minutes](https://learnxinyminutes.com/docs/yaml)
