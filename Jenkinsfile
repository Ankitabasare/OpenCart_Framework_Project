pipeline {

    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Maven project...'
                bat 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running TestNG test suite...'
                bat 'mvn test'
            }
        }
    }

    post {

        always {
            echo 'Publishing test results...'

            junit allowEmptyResults: true,
                  testResults: '**/target/surefire-reports/*.xml'

            archiveArtifacts artifacts: '''
                reports/**/*,
                screenshots/**/*,
                logs/**/*,
                target/surefire-reports/**/*
            ''',
            allowEmptyArchive: true
        }

        success {
            echo 'Build and tests completed successfully!'
        }

        failure {
            echo 'Build or tests failed. Please check the Jenkins console output and test reports.'
        }

        cleanup {
            echo 'Cleaning Jenkins workspace...'
        }
    }
}
