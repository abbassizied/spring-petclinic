 pipeline {
    agent any
  
    stages {
        stage('SCM') {
            steps {
                // git branch: 'main', url: 'https://github.com/abbassizied/spring-petclinic.git'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/abbassizied/spring-petclinic']])
                echo 'Git Checkout Completed'
            }
        }
        stage('Build') {
            steps {
                container ('maven'){
                  sh 'mvn version'
                }
            }   
            steps {
               sh "mvn clean package "
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                    sh 'mvn clean package sonar:sonar'
                    echo 'SonarQube Analysis Completed'
                }
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
