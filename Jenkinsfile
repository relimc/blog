pipeline {
    agent {
        docker 'node:14.16.0-alpine3.12'
    }
    stages {
        stage('Source') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '[myCredentials]', url: 'https://github.com/relimc/blog.git']]])
            }
        }
        stage('bulid') {
            steps {
                sh 'echo $(pwd)'
                sh 'npm install -g --registry=https://registry.npm.taobao.org hexo-cli'
                sh 'npm install --registry=https://registry.npm.taobao.org'
                sh 'hexo clean && hexo generate'
            }
        }
    }
}