# GitLab

**GitLab** is is an open source end-to-end software development platform with built-in version control, issue tracking, code review, CI/CD, and more.

## Table of contents

- [GitLab](#gitlab)
  - [Table of contents](#table-of-contents)
  - [CI/CD](#cicd)
    - [`.gitlab-ci.yml` file](#gitlab-ciyml-file)
      - [Keywords](#keywords)
    - [Runners](#runners)
    - [Jobs](#jobs)
      - [Hidden jobs](#hidden-jobs)
    - [Pipelines](#pipelines)
      - [Merge request pipelines](#merge-request-pipelines)
      - [Downstream pipelines](#downstream-pipelines)
    - [Stages](#stages)
    - [Variables](#variables)
    - [Caches](#caches)
    - [Artifacts](#artifacts)
      - [Job artifacts](#job-artifacts)
      - [Pipeline artifacts](#pipeline-artifacts)
    - [Services](#services)
    - [Docker](#docker)
      - [Run your CI/CD jobs in Docker containers](#run-your-cicd-jobs-in-docker-containers)
  - [GitLab Runner](#gitlab-runner)
    - [Runner registration](#runner-registration)
    - [Executors](#executors)
      - [Shell executor](#shell-executor)
      - [Docker executor](#docker-executor)
      - [Kubernetes executor](#kubernetes-executor)
      - [SSH executor](#ssh-executor)
      - [Custom executor](#custom-executor)
    - [Monitoring runners](#monitoring-runners)
  - [Releases](#releases)

## CI/CD

To use GitLab CI/CD:

- Ensure you have *runners* available to run your *jobs*.
- Create a `.gitlab-ci.yml` file at the root of your repository. This file is where you define your CI/CD jobs.

### `.gitlab-ci.yml` file

A **`.gitlab-ci.yml`** is a file that contains the CI/CD configuration.

The scripts of CI/CD  are grouped into *jobs*, and jobs run as part of a larger *pipeline*. You can group multiple independent jobs into *stages* that run in a defined order. The CI/CD configuration needs at least one job that is not hidden.

[↑ The `.gitlab-ci.yml` file](https://docs.gitlab.com/ee/ci/yaml/gitlab_ci_yaml.html).

[↑ Development guide for GitLab CI templates](https://gitlab.com/gitlab-org/gitlab/-/tree/master/lib/gitlab/ci/templates).

To use GitLab CI/CD, you need:

- Application code hosted in a Git repository.
- A file called `.gitlab-ci.yml` in the root of your repository, which contains the CI/CD configuration.

When you add a `.gitlab-ci.yml` file to your repository, GitLab detects it and an application called GitLab Runner runs the scripts defined in the jobs.

#### Keywords

A GitLab CI/CD pipeline configuration includes:

- Global keywords that configure pipeline behavior.
- Job keywords that configure jobs behavior

[↑ .gitlab-ci.yml keyword reference](https://docs.gitlab.com/ee/ci/yaml/).

### Runners

A **runner** is an agent that runs your CI/CD jobs.

### Jobs

A **job** is the instructions that a runner has to execute.

When you commit the file to your repository, the runner runs your jobs.

[↑ Jobs](https://docs.gitlab.com/ee/ci/jobs).

#### Hidden jobs

Start the job name with a dot `.` and it is not processed by GitLab CI/CD:

```yaml
.hidden_job:
  script:
    - run test
```

You can use hidden jobs that start with `.` as templates for reusable configuration with:

- The `extends` keyword.
- YAML anchors.

### Pipelines

A **pipeline** is a collection of jobs split into different *stages*.

[↑ Pipelines](https://docs.gitlab.com/ee/ci/pipelines/).

#### Merge request pipelines

You can configure your pipeline to run every time you commit changes to a branch. This type of pipeline is called a **branch pipeline**.

Alternatively, you can configure your pipeline to run every time you make changes to the source branch for a merge request. This type of pipeline is called a **merge request pipeline**.

Branch pipelines:

- Run when you push a new commit to a branch.
- Are the default type of pipeline.
- Have access to some predefined variables.
- Have access to protected variables and protected runners.

Merge request pipelines:

- Run when you:
  - Create a new merge request from a source branch with one or more commits.
  - Push a new commit to the source branch for a merge request.
  - Select **Run pipeline** from the **Pipelines** tab in a merge request. This option is only available  when merge request pipelines are configured for the pipeline and the source branch has at least  one commit.
- Do not run by default. The jobs in the CI/CD configuration file must be configured to run in - merge request pipelines.
- Have access to more predefined variables.
- Do not have access to protected variables or protected runners.

[↑ Merge request pipelines](https://docs.gitlab.com/ee/ci/pipelines/merge_request_pipelines.html).

#### Downstream pipelines

A **downstream pipeline** is any GitLab CI/CD pipeline triggered by another pipeline. Downstream pipelines run independently and concurrently to the upstream pipeline that triggered them.

A **parent-child pipeline** is a downstream pipeline triggered in the *same* project as the first pipeline.

A **multi-project pipeline** is a downstream pipeline triggered in a *different* project than the first pipeline.

[↑ Downstream pipelines](https://docs.gitlab.com/ee/ci/pipelines/downstream_pipelines.html).

### Stages

A **stage** is a keyword that defines certain stage of a job, such as `build` and `deploy`. Jobs of the same stage are executed in parallel.

Multiple jobs in the same stage are executed in parallel, if there are enough concurrent runners.

If *all* jobs in a stage succeed, the pipeline moves on to the next stage.

If *any* job in a stage fails, the next stage is not (usually) executed and the pipeline ends early.

### Variables

CI/CD variables are a type of environment variable. You can use them to:

- Control the behavior of jobs and pipelines.
- Store values you want to re-use.
- Avoid hard-coding values in your `.gitlab-ci.yml` file.

You can use [↑ predefined CI/CD variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html) or define custom.

[↑ GitLab CI/CD variables](https://docs.gitlab.com/ee/ci/variables).

### Caches

A **cache** is one or more files a job downloads and saves. Subsequent jobs that use the same cache don't have to download the files again, so they execute more quickly.

Use cache for dependencies, like packages you download from the internet. Cache is stored where GitLab Runner is installed, and uploaded to S3 if distributed cache is enabled.

Both caches and *artifacts* define their paths relative to the project directory, and can't link to files outside it.

[↑ Caching](https://docs.gitlab.com/ee/ci/caching).

As we know a pipeline is a set of stages and each stage can have one or more jobs. Jobs work on a distributed farm of runners. When we start a pipeline, a random runner with free resources executes the needed job. The GitLab-runner is the agent that can run jobs. For simplicity, let's consider Docker as an executor for all runners.

Each job starts with a clean slate and doesn't know the results of the previous one. If you don't use cache and artifacts, the runner will have to go to the internet or local registry and download the necessary packages when installing project dependencies.

[↑ GitLab CI: Cache and Artifacts explained by example](https://dev.to/drakulavich/gitlab-ci-cache-and-artifacts-explained-by-example-2opi).

### Artifacts

An **artifact** is a file created as part of a build process that often contain metadata about that build's jobs like test results, security scans, etc.

Use artifacts to pass intermediate build results between stages. Artifacts are generated by a job, stored in GitLab, and can be downloaded.

#### Job artifacts

Jobs can output an archive of files and directories. This output is known as a **job artifact**.

#### Pipeline artifacts

A **pipeline artifact** is a file created by GitLab after a pipeline finishes. Pipeline artifacts are different to *job artifacts* because they are not explicitly managed by .gitlab-ci.yml definitions.

Pipeline artifacts are used by the test coverage visualization feature to collect coverage information.

Pipeline artifacts from the latest pipeline are kept forever. Artifacts from pipelines superseded by a newer pipeline are deleted seven days after their creation date. It can take up to two days for GitLab to delete pipeline artifacts from when they are due to be deleted.

### Services

The `services` keyword defines a Docker image that runs during a job linked to the Docker image that the image keyword defines. This allows you to access the service image during build time.

The service image can run any application, but the most common use case is to run a database container, for example:

- MySQL
- PostgreSQL
- Redis

[↑ Services](https://docs.gitlab.com/ee/ci/services).

### Docker

There are two primary ways to incorporate Docker into your CI/CD workflow:

- Run your CI/CD jobs in Docker containers.

You can create CI/CD jobs to do things like test, build, or publish an application. These jobs can run in Docker containers.

For example, you can tell GitLab CI/CD to use a Node image that's hosted on Docker Hub or in the GitLab Container Registry. Your job then runs in a container that's based on the image. The container has all the Node dependencies you need to build your app.

- Use Docker or kaniko to build Docker images.

You can create CI/CD jobs to build Docker images and publish them to a container registry.

#### Run your CI/CD jobs in Docker containers

You can run your CI/CD jobs in separate, isolated Docker containers.

If you run Docker on your local machine, you can run tests in the container, rather than testing on a dedicated CI/CD server.

To run CI/CD jobs in a Docker container, you need to:

- Register a runner so that all jobs run in Docker containers. Do this by choosing the Docker executor during registration.
- Specify which container to run the jobs in. Do this by specifying an `image` in your `.gitlab-ci.yml` file.
- Optional. Run other services, like MySQL, in containers. Do this by specifying `services` in your `.gitlab-ci.yml` file.

## GitLab Runner

**GitLab Runner** is an application that works with GitLab CI/CD to run jobs in a pipeline.

You can choose to install the GitLab Runner application on infrastructure that you own or manage. If you do, you should install GitLab Runner on a machine that's separate from the one that hosts the GitLab instance for security and performance reasons. When you use separate machines, you can have different operating systems and tools, like Kubernetes or Docker, on each.

GitLab Runner is open-source and written in Go. It can be run as a single binary; no language-specific requirements are needed.

You can install GitLab Runner on several different supported operating systems. Other operating systems may also work, as long as you can compile a Go binary on them.

GitLab Runner can also run inside a Docker container or be deployed into a Kubernetes cluster.

### Runner registration

After you install the application, you register individual runners. Runners are the agents that run the CI/CD jobs that come from GitLab.

When you register a runner, you are setting up communication between your GitLab instance and the machine where GitLab Runner is installed.

Runners usually process jobs on the same machine where you installed GitLab Runner. However, you can also have a runner process jobs in a container, in a Kubernetes cluster, or in auto-scaled instances in the cloud.

### Executors

When you register a runner, you must choose an *executor*.

An **executor** determines the environment each job runs in.

For example:

- If you want your CI/CD job to run PowerShell commands, you might install GitLab Runner on a Windows server and then register a runner that uses the shell executor.
- If you want your CI/CD job to run commands in a custom Docker container, you might install GitLab Runner on a Linux server and register a runner that uses the Docker executor.

These are only a few of the possible configurations. You can install GitLab Runner on a virtual machine and have it use another virtual machine as an executor.

When you install GitLab Runner in a Docker container and choose the Docker executor to run your jobs, it's sometimes referred to as a "Docker-in-Docker" configuration.

GitLab Runner implements a number of executors that can be used to run your builds in different scenarios.

- SSH
- Shell
- Parallels
- VirtualBox
- Docker
- Kubernetes
- Custom

#### Shell executor

Shell is the simplest executor to configure. All required dependencies for your builds need to be installed manually on the same machine that GitLab Runner is installed on.

#### Docker executor

A great option is to use Docker as it allows a clean build environment, with easy dependency management (all dependencies for building the project can be put in the Docker image). The Docker executor allows you to easily create a build environment with dependent services, like MySQL.

#### Kubernetes executor

The Kubernetes executor allows you to use an existing Kubernetes cluster for your builds. The executor will call the Kubernetes cluster API and create a new Pod (with a build container and services containers) for each GitLab CI job.

#### SSH executor

The SSH executor is added for completeness, but it's the least supported among all executors. It makes GitLab Runner connect to an external server and runs the builds there. We have some success stories from organizations using this executor, but usually we recommend using one of the other types.

#### Custom executor

The Custom executor allows you to specify your own execution environments. When GitLab Runner does not provide an executor (for example, LXC containers), you are able to provide your own executables to GitLab Runner to provision and clean up any environment you want to use.

[↑ Executors](https://docs.gitlab.com/runner/executors).

### Monitoring runners

You can use Prometheus to monitor your runners. You can view things like the number of currently-running jobs and how much CPU your runners are using.

[↑ GitLab Runner](https://docs.gitlab.com/runner).

## Releases

[↑ Releases](https://docs.gitlab.com/ee/user/project/releases/).
