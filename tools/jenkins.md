# Jenkins

## Integration with GitLab

<https://docs.gitlab.com/ee/integration/jenkins.html>

## `Jenkinsfile`

Example of `Jenkinsfile`:

```groovy
pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                sh 'env'
                echo 'Printing working directory'
                pwd()
                echo "${JENKINS_URL}"
            }
        }        
    }
}
```
