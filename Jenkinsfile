 pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    	
    stages {
        stage('SCM') {
            steps {
                # git branch: 'main', url: 'https://github.com/abbassizied/spring-petclinic.git'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/abbassizied/spring-petclinic']])
                echo 'Git Checkout Completed'
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('ServerNameSonar') {
                    sh '''mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=spring-petclinic \
                          -Dsonar.projectName='spring-petclinic' \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.token=sqp_27d348d522e1a5e98c64e1be6c9f7288e45f915b 
                       '''
                    echo 'SonarQube Analysis Completed'
                }
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
