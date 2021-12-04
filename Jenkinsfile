pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    
    stages {

        stage('Cleanup') {
            steps {
                // Clean workspace before build
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                script {
                    def gitRemoteOriginUrl = scm.getUserRemoteConfigs()[0].getUrl()
                    echo 'The remote URL is ' + gitRemoteOriginUrl
                    scmVars = checkout([$class: 'GitSCM', branches: [[name: 'refs/heads/$BRANCH_NAME']], extensions: [[$class: 'GitLFSPull'],[$class: 'LocalBranch', localBranch: '**']], gitTool: 'git', userRemoteConfigs: [[credentialsId: "GitLab", url: gitRemoteOriginUrl]]])
                }
            }
        }

        stage('Build') {
            steps {
                withMaven(jdk: 'JDK11', maven: 'Maven-3.8.4') {
                    sh "mvn clean verify"
                }
            }
        }

    }

}
