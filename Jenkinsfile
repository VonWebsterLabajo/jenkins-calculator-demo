pipeline {
  agent any
  tools {
    jdk 'jdk21'
    maven 'maven3'
  }

  triggers {
    githubPush()
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/VonWebsterLabajo/jenkins-calculator-demo.git'
      }
    }

    stage('Build App') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Archive Build Artifact') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Trigger Test Pipeline') {
      steps {
        build job: 'RepoB_Test_Pipeline', 
          parameters: [
            string(name: 'APP_VERSION', value: "${env.BUILD_NUMBER}"),
            string(name: 'ARTIFACT_URL', value: "${env.BUILD_URL}artifact/target/app.jar")
          ],
          wait: false
      }
    }
  }
}