pipeline {
    agent any
    environment {
        DEPLOY_ENV = 'staging'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/vaix-ducvn/jenkins_docker-fork.git',
                    credentialsId: 'github-credentials'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        // stage('Lint & Format') {
        //     steps {
        //         sh 'npx eslint .'
        //         sh 'npx prettier --check .'
        //     }
        // }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'cp -r dist/* /var/jenkins_home/dist'
            }
        }
    }
    post {
        success {
            script {
                def commitMsg = sh(script: "git log -n 1 --pretty=format:'%h - %s (%an)'", returnStdout: true).trim()
                slackSend channel: '#testing-chat-bot',
                          color: 'good',
                          message: """Build *${env.JOB_NAME} #${env.BUILD_NUMBER}* thành công! :tada:
- Deployed to: ${env.DEPLOY_ENV}
- Thời gian: ${currentBuild.durationString}
- Commit: `${commitMsg}`
- Xem chi tiết: ${env.BUILD_URL}""",
                          teamDomain: 'testing-chat-bothq',
                          tokenCredentialId: 'slack-token'
            }
        }
        failure {
            script {
                def commitMsg = sh(script: "git log -n 1 --pretty=format:'%h - %s (%an)'", returnStdout: true).trim()
                slackSend channel: '#testing-chat-bot',
                          color: 'danger',
                          message: """Build *${env.JOB_NAME} #${env.BUILD_NUMBER}* thất bại! :x:
- Thời gian: ${currentBuild.durationString}
- Commit: `${commitMsg}`
- Xem log chi tiết: ${env.BUILD_URL}""",
                          teamDomain: 'testing-chat-bothq',
                          tokenCredentialId: 'slack-token'
            }
        }
    }
}
