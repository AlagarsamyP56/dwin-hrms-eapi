pipeline {
    agent any

    environment {
        ANYPOINT_USERNAME = credentials(pipeline {
    agent any

    environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.hrms')
        ANYPOINT_USERNAME = credentials('ANYPOINT_USERNAME') // Update with the correct ID
        ANYPOINT_PASSWORD = credentials('ANYPOINT_PASSWORD') // Update with the correct ID
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
            environment {
                ANYPOINT_CREDENTIALS = credentials('anypoint.hrms')
            }
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
) // Update with the correct ID
        ANYPOINT_PASSWORD = credentials('Good@123') // Update with the correct ID
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
            environment {
                ANYPOINT_CREDENTIALS = credentials('anypoint.hrms')
            }
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
