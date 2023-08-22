pipeline {
  agent any 
  tools {
    maven 'M3'
    jdk 'JDK11'
  }
  environment {
    AWS_CREDENTIALS_NAME = "AWSCredentials"
    REGION = "ap-northeast-2"
    DOCKER_IMAGE_NAME = "project04-spring-petclinic"
    DOCKER_TAG = "1.0"
    ECR_REPOSITORY = "257307634175.dkr.ecr.ap-northeast-2.amazonaws.com"
    ECR_DOCKER_IMAGE = "${ECR_REPOSITORY}/${DOCKER_IMAGE_NAME}"
    ECR_DOCKER_TAG = "${DOCKER_TAG}" 
    APPLICATION_NAME = "project04-production-in-place"
    DEPLOYMENT_GROUP_NAME = "project04-production-in-place"
    AUTO_SCALING_GROUP_NAME = "project04-target-group"
    SERVICE_ROLE_ARN = "arn:aws:iam::257307634175:role/project04-code-deploy-service-role"
    DEPLOYMENT_CONFIG_NAME = "CodeDeployDefault.OneAtATime"
    S3_BUCKET = "project04-terraform-state"
  }
  
  stages {
    stage('Git Clone') {
      steps {
        git url: 'https://github.com/alltelier/spring-petclinic.git', branch: 'efficient-webjars-1', credentialsId: 'gitCredentials'
      }
    }
    stage('mvn build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true install'
      }
      post {
        success {
          junit '**/target/surefire-reports/TEST-*.xml'
        }
      }
    }
    stage('Docker Image Build') {
      steps {
        dir("${env.WORKSPACE}") {
          sh 'docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} .'
        }
      }
    }
  
  }
}
