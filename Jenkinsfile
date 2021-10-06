pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: 'maven:3.8.2-jdk-11'
    command:
    - cat
    tty: true
    '''
  }
}
    tools{
        maven 'maven 3'
    }
    stages {
        stage ("initialize") {
            steps {
                sh '''
                echo "PATH = ${PATH}"
                echo "M3_HOME = ${M3_HOME}"
                '''
            }
        }
        stage ('build') {
            steps {
                sh 'mvn clean package'
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