 pipeline {
    agent any 
  
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    	
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/abbassizied/spring-petclinic.git'
            }
        }
        stage('Sonarqube Analysis') {
                   def mvn = tool 'Default Maven';
                   withSonarQubeEnv() {
                     sh '''${mvn}/bin/mvn clean verify sonar:sonar \
                           -Dsonar.projectKey=spring-petclinic \
                           -Dsonar.projectName='spring-petclinic' \
                           -Dsonar.host.url=http://localhost:9000 \
                           -Dsonar.token=sqp_27d348d522e1a5e98c64e1be6c9f7288e45f915b 
                     '''
                   }
        }
        stage('Build') {
            steps {
               sh "mvn clean package "
            }
        }
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan target/', odcInstallation: 'owasp'
            }
        }
        stage('Publish OWASP Dependency Check Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'OWASP Dependency Check Report'
                ])
            }
        }
        stage('What\'s next?') {
            steps {
                echo 'What\'s next?' 
            }
        }
    }
}    
