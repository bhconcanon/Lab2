pipeline {
  agent any
    tools{
        maven 'maven 3'
    }
    stages {
        stage ("initialize") {
            steps {
                sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
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
                sh './deliver.sh'
                }
        }
    }
}  