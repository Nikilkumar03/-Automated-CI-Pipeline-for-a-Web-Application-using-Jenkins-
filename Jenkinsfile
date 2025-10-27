pipeline {
  agent any
  environment {
    BUILD_DIR = "build"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Install') {
      steps {
        sh 'npm ci'          // use `pip install -r requirements.txt` or `mvn -B -DskipTests install`
      }
    }
    stage('Static Analysis (Optional)') {
      steps {
        script {
          // Example: npm run lint (add eslint if you want)
          sh 'echo "Skipping lint (optional) - add linter if needed"'
        }
      }
    }
    stage('Unit Tests') {
      steps {
        sh 'npm test'
      }
      post {
        always {
          junit '**/junit*.xml'   // collects test reports (from jest-junit)
        }
        failure {
          echo 'Tests failed.'
        }
      }
    }
    stage('Build & Package') {
      steps {
        sh 'mkdir -p $BUILD_DIR'
        sh 'npm run build'
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }
  post {
    success {
      mail to: 'dev-team@example.com',
           subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Good news — build succeeded. Artifacts archived."
    }
    failure {
      mail to: 'dev-team@example.com',
           subject: "Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Build failed. Check console output."
    }
  }
}
