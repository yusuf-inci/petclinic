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
                //git branch: 'main', url: 'https://github.com/jaiswaladi246/Petclinic.git'

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
                    $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://192.168.50.10:9000/ -Dsonar.login=squ_c3b4d923a520c5a7087301f952b54874d956bd2e\
                    -Dsonar.projectName=petclinic \
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
        
        // stage('OWASP Dependency Check') {
            // steps {
                // dependencyCheck additionalArguments: '--scan target/', odcInstallation: 'owasp'
            // }
        // }
        
        // stage('Publish OWASP Dependency Check Report') {
            // steps {
                // publishHTML(target: [
                    // allowMissing: false,
                    // alwaysLinkToLastBuild: true,
                    // keepAll: true,
                    // reportDir: 'target',
                    // reportFiles: 'dependency-check-report.html',
                    // reportName: 'OWASP Dependency Check Report'
                // ])
            // }
        // }

        // stage('Deploy') {
            // steps {
                // sh "sudo cp target/*.war /opt/apache-tomcat-9.0.65/webapps"
            // }
        // }
    }
}
