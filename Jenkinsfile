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
                git branch: 'main', url: 'https://github.com/yusuf-inci/petclinic.git'

            }
        }

        stage('Compile') {
            steps{
                sh "mvn clean compile"
            }
        }

        stage('Test Cases') {
            steps{
                sh "mvn test"
            }
        }

        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' 
                    $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=petclinic \
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

        // stage('Deploy') {
            // steps {
                // sh "sudo cp target/*.war /opt/apache-tomcat-9.0.65/webapps"
            // }
        // }
    }
}
