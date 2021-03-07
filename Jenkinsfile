pipeline {
    agent {
        docker 'node:14.16.0-alpine3.12'
    }
    stages {
        stage('Source') {
            steps {
                git 'https://github.com/relimc/blog.git'
            }
        }
        stage('bulid') {
            steps {
                sh 'npm install -g --registry=https://registry.npm.taobao.org hexo-cli'
                sh 'npm install --registry=https://registry.npm.taobao.org'
                sh 'hexo clean && hexo generate'
            }
        }
    }
}