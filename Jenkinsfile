pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
    DOCKER_IMAGE = "testapp"
  }
   stages {
       stage('Checkout Code') {
       steps {
           // Realiza checkout del código desde GitHub
           git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/czambrano1997/mundosE-Pin1.git'
       }
   }

   stage('Build image docker') {
       steps {
                script {
                    sh '''
                    cd webapp
                    docker build -t ${DOCKER_IMAGE} .
                    '''
                }
        }
    }


    stage('Run tests') {
      agent {
        docker {
          image 'node:alpine'
        }
      }
      steps {
        sh "npm test"
      }
    }

    stage('Deploy Image') {
      steps{
        sh '''
        docker tag ${DOCKER_IMAGE} 127.0.0.1:5000/pin1_grupo7/webapp
        docker push 127.0.0.1:5000/pin1_grupo7/webapp
        '''
        }
      }
    }
}

