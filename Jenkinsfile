pipeline{
    agent any

    tools{
        jdk 'jdk11'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages{
        stage('Git Checkout') {
            steps{
                git branch: 'main', url: 'https://github.com/yusuf-inci/30.Days.of.DevOps.git'

            }
        }

        stage('Compile') {
            steps{
                sh "mvn clean compile"
            }
        }

        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv(sonar-server) {
                    sh ''' 
                    $SCANNER_HOME/bin/sonar-server -Dsonar.projectName=petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=petclinic '''
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
    }
}
