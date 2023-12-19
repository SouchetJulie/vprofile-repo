pipeline {
    agent any

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
