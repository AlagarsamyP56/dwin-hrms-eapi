pipeline {
    agent any
environment {
        M2_HOME = '/opt/apache-maven-3.9.8'
        PATH = "${env.PATH}:${env.M2_HOME}/bin"
    }
    stages {
        stage('Build Application') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        stage('Deploy to Docker') {
            steps {
                script {
                    // Get the JAR file path using a Linux-compatible command
                    def jarPath = sh(script: 'ls target/*.jar | head -n 1', returnStdout: true).trim()
                    
                    echo "Verified JAR file path: ${jarPath}"

                    def containerName = env.JOB_NAME
                    echo "Docker container name: ${containerName}"

                    // Stop and remove existing container
                    sh "docker stop ${containerName} || true"
                    sh "docker rm ${containerName} || true"

                    // Run the Docker container
                    sh "docker run -d --name ${containerName} -p 8088:8081 dockermule"

                    // Print a message indicating that the JAR file will be copied
                    echo "Copying JAR file to Docker container: ${jarPath}"

                    // Copy the JAR file to the Docker container
                    sh "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"
                }
            }
        }
    }
}

