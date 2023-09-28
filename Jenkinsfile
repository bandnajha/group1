pipeline {
    agent any
    tools {
        maven 'maven-3.9.4'
    }
    stages {
        stage('GIT CHECKOUT') {
            steps {
                git branch: 'main', url: 'https://github.com/bandnajha/group1.git'
            }
        }
         stage('BUILD') {
            steps {
                sh 'mvn clean install package'
            }
        }
         stage('LOGIN INTO ECR') {
            steps {
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/f4o2p1q4'
            }
        }
         stage('DOCKER IMAGE BUILD') {
            steps {
                sh 'docker build -t mypipelineproject .'
            }
        }
        stage('DOCKER Push') {
            steps {
                sh 'docker tag mypipelineproject:latest public.ecr.aws/z4z2f9f3/mypipelineproject:latest'
                sh 'docker push public.ecr.aws/z4z2f9f3/mypipelineproject:latest'
            }
        }
        stage('DOCKER DEPLOY') {
            steps {
                sh 'docker run -d -p 4000:8080 public.ecr.aws/z4z2f9f3/mypipelineproject:latest'
            }
        }
    }
