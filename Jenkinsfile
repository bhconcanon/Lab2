pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: build
    image: 'maven:3.5.4-jdk-8'
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
                sh 'mvn -B -DskipTests clean package'
            }
    }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage ('deploy') {
            steps {
                sh './scripts/deliver.sh'
                }
        }
    }
        post {
            success {
                emailext (
                    subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """SUCCESSFUL: Job '${JOB_NAME} [${BUILD_NUMBER}]':
                    Check console output at ${BUILD_URL}""",
                    to: 'bilal.hussain@concanon.com'
                )
            }
            failure {
                emailext (
                    subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                    body: """FAULURE: Job '${JOB_NAME} [${BUILD_NUMBER}]':
                    Check console output at ${BUILD_URL}""",
                    to: 'bilal.hussain@concanon.com'
            )
        }    
    }
}