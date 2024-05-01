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
   
   stage('Change to Webapp Directory') {
        steps {
                script {
                    // Cambia al directorio 'webapp' dentro del repositorio clonado
                    dir('webapp') {
                        // Construye la imagen Docker en este subdirectorio
                        docker.build("${env.DOCKER_IMAGE}")
                    }
                }
        }
    }
   

    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
    
    stage('Deploy Image') {
      steps{
        sh '''
        docker tag testapp 127.0.0.1:5000/pin1_grupo7/webapp
        docker push 127.0.0.1:5000/pin1_grupo7/webapp   
        '''
        }
      }
    }
}

