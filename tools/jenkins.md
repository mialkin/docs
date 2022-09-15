# Jenkins

- [Jenkins](#jenkins)
  - [Installation](#installation)
  - [Jenkins Pipeline](#jenkins-pipeline)
  - [Remote Access Via SSH](#remote-access-via-ssh)

## Installation

```bash
docker run \
-u root \
-d \
-p 8080:8080 \
--name jenkins-blueocean \
-v /Users/aleksei/jenkins_home:/var/jenkins_home \
-v /Users/aleksei/.nuget/packages:/var/jenkins_home/.nuget/packages \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkinsci/blueocean
```

## Jenkins Pipeline

A **Jenkins Pipeline** (or simply "Pipeline" with a capital "P") is a suite of plugins which supports implementing and integrating _continuous delivery pipelines_ into Jenkins.

A **continuous delivery pipeline** is an automated expression of your process for getting software from version control right through to your users and customers.

Typically, the definition of a Jenkins Pipeline is written into a text file (called a `Jenkinsfile`) which in turn is checked into a project's source control repository. Example of a `Jenkinsfile`:

```groovy
// Declarative pipeline
pipeline {
    agent any
    stages {
        stage('Go') {
            steps {
                sh 'env'
                echo 'Printing working directory'
                pwd()
                echo "${JENKINS_URL}"
            }
        }
    }
}
// Scripted pipeline
node {
    stage('Go') {
        sh 'env'
        echo 'Printing working directory'
        pwd()
        echo "${JENKINS_URL}"
    }
}
```

A **step** is a single task; fundamentally steps tell Jenkins what to do.

A **stage** is a step for defining a conceptually distinct subset of the entire Pipeline, for example: "Build", "Test", and "Deploy", which is used by many plugins to visualize or present Jenkins Pipeline status/progress.

## Remote Access Via SSH

```groovy
pipeline {
    agent any
    stages {
        stage('Go') {
            steps {
                sshagent(credentials: ['YOUR_CREDENTIALS_ID']) {
                    sh '''
                        [ -d ~/.ssh ] || mkdir ~/.ssh && chmod 0700 ~/.ssh
                        ssh-keyscan -t rsa,dsa hostname-or-ip-address >> ~/.ssh/known_hosts
                    '''
                    sh '''ssh -tt username@hostname-or-ip-address << EOF
                    kubectl get pods -n mialkin
                    exit
                    EOF'''
                }
            }
        }
    }
}
```
