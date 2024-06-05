 pipeline {
   agent any
   tools {
     maven 'maven397'
   }
   stages {
     stage('SCM') {
       steps {
         // git branch: 'main', url: 'https://github.com/abbassizied/spring-petclinic.git'
         checkout scmGit(branches: [
           [name: '*/main']
         ], extensions: [], userRemoteConfigs: [
           [url: 'https://github.com/abbassizied/spring-petclinic']
         ])
         echo 'Git Checkout Completed'
       }
     }

     stage('Build') {
       steps {
         sh 'mvn clean install'
       }
     }

     stage('Sonarqube Analysis') {
       steps {
         withSonarQubeEnv(installationName: 'sq1') {
           sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121:sonar'
         }
       }
     }

        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan target/', odcInstallation: 'owasp-dc'
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
