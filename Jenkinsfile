pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: build
    image: 'maven:3.5.4-jdk-8-slim'
    command:
    - cat
    tty: true
    '''
            defaultContainer 'build'
  }
}
    tools{
        maven 'maven 3'
    }
    stages {
        stage ('build') {
            steps {
                sh 'mvn -X clean package'
            }
    }
        stage ('test') {
            steps {
                sh 'mvn test'
            }
    }
        stage ('deploy') {
            steps {
                sh './scripts/deliver.sh'
                }
        }
        stage('Email') {
        steps {
          emailext (
            subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """SUCCESSFUL: Job '${JOB_NAME} [${BUILD_NUMBER}]':
            Check console output at ${BUILD_URL}""",
            to: 'bilal.hussain@concanon.com'
                )
            }       
        }
    }
}  