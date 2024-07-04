pipeline {
    agent any

    environment {
        ANYPOINT_USERNAME = credentials('alagar123456') // Update with the correct ID
        ANYPOINT_PASSWORD = credentials('eyJhbGciOiJFUzI1NiIsInR5cCI6Imp3dCIsImtpZCI6ImFueXBvaW50X2lhbV9wcm9kLWMyYy00LTE3MTg0MDk2MjE4MzciLCJ2ZXIiOiIxLjAifQ.eyJhdXQiOiJTRVJWSUNFIiwiY3R4Ijoic2ZkYy5naWQtYXV0aCIsImlzdCI6Miwic3R5IjoiVGVuYW50Iiwic2NvcGUiOiJ2ZXJpZnkiLCJ2YWFzX2FyZ3MiOnsiYWN0aW9uX2lkIjoibG9naW4iLCJhY3Rpb25fbmFtZSI6IkxvZ2luIiwidHJ1c3RfdmVyaWZpZWRfZGV2aWNlcyI6ZmFsc2UsImVtYWlsIjoiIiwicmVkaXJlY3RfdXJpIjoiaHR0cHM6Ly9hbnlwb2ludC5tdWxlc29mdC5jb20vYWNjb3VudHMvbG9naW4vbWZhX2NhbGxiYWNrIiwic3RhdGUiOiI2MDU0NmZkOC0xZTQxLTQzYjgtOGMzMi05MWE2ZmMwOGIyNmUiLCJ1c2VyX2FjY291bnRfaWQiOiIzMmIxMWFkZi1mNTNmLTQzZjMtYmQ3NC1mNGRjNmY4NjU1ZjgiLCJ1c2VyX2Rpc3BsYXlfbmFtZSI6ImFsYWdhcjEyMzQ1NiJ9LCJpYXQiOjE3MjAwOTI0NTgsIm5iZiI6MTcyMDA5MjQ1OCwiZXhwIjoxNzIwMDkyNDc4LCJhdWQiOiJ2YWFzIiwiaXNzIjoiYW55cG9pbnRfYWNjZXNzX21hbmFnZW1lbnRfdXMxIiwianRpIjoiYmI2NDhmYWEtY2Q0My00N2Y3LTg1MWEtOTk5ODJmNzNlZTAxIn0.P_7OvxBrwCxqPl7MwIu4_fwyeE3R9cw17EClfCdXBtFHeM5aZ5Mhl4blt0BPkfoIMIhifgwEr-FxbgKBlBctRg') // Update with the correct ID
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
