pipeline {
    agent any

    environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.hrms')
    }

    stages {
        stage('Build Application') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }
        stage('Deploy CloudHubs') {
            steps {
                echo 'Deploying mule project due to the latest code commit…'
                echo 'Deploying to the configured environment….'
                script {
                    withCredentials([usernamePassword(credentialsId: 'anypoint.hrms', usernameVariable: 'ANYPOINT_USERNAME', passwordVariable: 'ANYPOINT_PASSWORD')]) {
                        if (isUnix()) {
                            sh "mvn clean deploy -DmuleDeploy"
                        } else {
                            bat "mvn clean deploy -DmuleDeploy"
                        }
                    }
                }
            }
        }
    }
}
