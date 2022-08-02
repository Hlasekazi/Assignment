# Assignment
Click to Jenkins -> new Item 
when I click on OK, the description and writing page of our pipeline script is displayed.
 I place  a description in the description section of the page that appeared when I click on Ok button. 
 I scelected the pipeline
 I created a Jenkinsfile below
 
pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '076892551558.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
    registryCredential = 'jenkins-ecr'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'master', url: 'https://github.com/Hlasekazi/helloworld_jan_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
