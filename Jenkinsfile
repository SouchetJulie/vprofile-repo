pipeline {
  agent {
    docker {
      image 'maven:3.9.6-eclipse-temurin-11'
      args '-v /root/.m2:/root/.m2'
    }
  }

  stages {
    stage('fetch code') {
      steps {
        git branch: 'master', url: "https://github.com/souchetjulie/vprofile-repo.git"
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

    stage("UploadArtifact") {
      steps {
        nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: 'adt-nexus.easyvista-training.com:8081',
          groupId: 'adt-js-maven-group',
          version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
          repository: 'adt-js-maven',
          credentialsId: 'nexus-login',
          artifacts: [[
            artifactId: 'vproapp',
            classifier: '',
            file: 'target/vprofile-v1.war',
            type: 'war'
          ]]
        )
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Code Analysis') {
      steps {
        sh 'mvn checkstyle:checkstyle'
      }
    }
  }
}
