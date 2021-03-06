pipeline {
    agent {
        docker 'node:14.16.0-alpine3.12'
    }
    triggers {
        githubpush()
    }
    stages {
        stage('Source') {
            steps {
                sh 'mkdir -p /home/code && cd /home/code'
                git 'https://github.com/relimc/blog.git'
            }
        }
        stage('bulid') {
            steps {
                sh 'cd /home/code/blog'
                sh 'npm install --registry=https://registry.npm.taobao.org hexo-cli'
                sh 'npm install --registry=https://registry.npm.taobao.org'
                sh 'hexo clean && hexo generate'
            }
        }
    }
}