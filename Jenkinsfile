pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkins-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Update code from SCM ${env.BUILD_ID} on ${env.JENKINS_URL}..'
                checkout scm
                sh "echo hello"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                echo 'Building....'
                sh 'npm run build'
            }
        }
        stages {
            stage('Deploy') {
                when {
                  expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                  }
                }
                steps {
                    sh 'aws s3 cp  dist/ s3://test/'
                }
            }
    }
}
