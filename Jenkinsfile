pipeline {
    agent {
      docker {
        image 'maven:3.9.6-eclipse-temurin-11'
        args '-v /root/.m2:/root/.m2'
      }
    }

    stages{
        stage('fetch code') {
          steps{
              git branch: 'vp-rem', url: "https://github.com/souchetjulie/vprofile-repo.git"
          }  
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }
         stage('Code Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
    }
}
