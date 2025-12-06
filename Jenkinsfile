pipeline {
    agent any
    environment {
      NETLIFY_SITE_ID = "a369469b-dddd-4e07-adda-ff667f4b6569"
      NETLIFY_AUTH_TOKEN = credentials('netlify-creds')
    }

    stages {
        stage('Build') {
          agent {
            docker {
              image 'node:18-slim'
              reuseNode true
            }
          }
          steps {
            sh '''
                ls -la
                whoami
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
            '''
          }
        }

        // stage('Test'){
        //   agent {
        //     docker {
        //       image 'node:18-alpine'
        //       reuseNode true
        //     }
        //   }
        //   steps {
        //     sh '''
        //       test -f build/index.html
        //       npm test
        //     '''
        //   }
        // }
        stage('Deploy') {
          agent {
            docker {
              image 'node:18-slim'
              reuseNode true
            }
          }
          steps {
            sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod
            '''
          }
        }
    }
    
    post {
        always {
          junit 'test-results/junit.xml'
        }
  }
}