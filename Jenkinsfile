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

     stage('Build du Projet') {
       steps {
         sh 'mvn clean install'
       }
     }

     stage('Analyse Sonarqube') {
       steps {
         withSonarQubeEnv(installationName: 'sq1') {
           sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121:sonar'
         }
       }
     }

     stage('OWASP Dependency-Check Vulnerabilities') {
       steps {
         dependencyCheck additionalArguments: ''
         '  -
         o './' -
           s './' -
           f 'ALL'
           --prettyPrint ''
         ', odcInstallation: '
         OWASP Dependency - Check Vulnerabilities '

         dependencyCheckPublisher pattern: 'dependency-check-report.xml'
       }
     }

     stage('What\'s next?') {
       steps {
         echo 'What\'s next?'
       }
     }
   }
 }
