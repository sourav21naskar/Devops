pipeline
{
    agent{label 'Jenkins-Agent'}
   tools
  {
      jdk 'java17'
     maven 'Maven3'
  }
    environment 
    {
        APP_NAME = "DOCKER"
        RELEASE = "1.0.0"
        DOCKER_USER = "sourev21naskar"
        DOCKER_PASS = "Sourav@21"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
  stages
  {
    stage("Cleanup Workspace")
    {
      steps
      {
        cleanWs()
      }
    }
    stage("Checkout from SCM")
    {
      steps
        {  
          git branch:'main',credentialsId: 'github', url: 'https://github.com/sourav21naskar/Devops.git'
        }
    }
    stage("Build Application")
    {
      steps
        {  
          sh "mvn clean package"
         }
    }
      stage("Test Application")
      {
        steps
          {  
                sh "mvn test"
          }
      }
      stage("Build & Push Docker Image") {
             steps {
                 script {
                     docker.withRegistry('',DOCKER_PASS) {
                         docker_image = docker.build "${IMAGE_NAME}"
                     }
                     docker.withRegistry('',DOCKER_PASS) {
                         docker_image.push("${IMAGE_TAG}")
                         docker_image.push('latest')
                     }
                 }
             }
         }
    }
  }

