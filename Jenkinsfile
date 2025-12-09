pipeline {
  agent any

  tools {
    // Ensure these tool names match Jenkins Global Tool Configuration
    jdk 'jdk17'
    maven 'maven-latest'
  }

  environment {
    MAVEN_OPTS = '-Dmaven.test.failure.ignore=false'
  }

  stages {
    stage('Checkout') {
      steps {
        // When running "Pipeline script from SCM", Jenkins provides scm binding
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -B clean package'
      }
    }

    stage('Archive Artifacts') {
      steps {
        archiveArtifacts artifacts: 'target/**/*.jar', fingerprint: true
      }
    }

    stage('Test Reports') {
      steps {
        junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}

