pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "bjumbo/pin1_grupo7:${env.BUILD_NUMBER}"
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
      steps {
        sh "docker run --rm ${DOCKER_IMAGE} npm test"
      }
    }

    stage('Deploy Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          sh '''
          set +x
          echo ${DOCKER_PASSWORD} | docker login --username ${DOCKER_USERNAME} --password-stdin
          if [ $? -eq 0 ]; then
            docker tag ${DOCKER_IMAGE} ${ARTIFACT_ID}
            docker push ${ARTIFACT_ID}
          else
            echo "Error: Docker login failed"
            exit 1
          fi
          set -x
          '''
        }
      }
    }

    stage('Scan Image') {
      steps {
        sh "docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --severity=CRITICAL ${ARTIFACT_ID}"
      }
    }
  }
}
