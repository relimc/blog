pipeline {
    agent {
        docker 'node:14.16.0-alpine3.12'
    }
    stages {
        stage('Source') {
            steps {
                git 'https://gitee.com/toadlive/blog.git'
            }
        }
        stage('bulid') {
            steps {
                sh 'npm install --registry=https://registry.npm.taobao.org hexo-cli'
                sh 'npm install --registry=https://registry.npm.taobao.org'
                sh 'hexo clean && hexo generate'
            }
        }
    }
}