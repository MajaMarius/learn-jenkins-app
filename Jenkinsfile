pipeline {
  agent any

  stages {
    stage('Build') {
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          set -eux
          ls -la
          node --version
          npm --version

          npm install
          npm run build
        '''
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          set -eux
          test -f build/index.html
          npm test -- --watchAll=false
        '''
      }
    }
  }

  post {
    always {
      junit allowEmptyResults: true, testResults: 'test-results/junit.xml'
    }
  }
}
