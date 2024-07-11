pipeline {
    agent any

    environment {
        PORTAINER_URL = 'https://192.168.0.70:9443/api'
        PORTAINER_TOKEN = 'ptr_efjRejFgPD9fvHMT3DgnGFL9pUGZvAbt6+JliABIlfE='
    }

    stages {
        stage('Build Application') {
            steps {
                script {
                    bat 'mvn clean install'
                }
            }
        }
        stage('Deploy to Docker using Portainer') {
            steps {
                script {
                    // Get the JAR file path using PowerShell on Windows
                    def jarPath = bat(script: 'powershell -Command "Get-ChildItem target\\*.jar | Select-Object -First 1 | ForEach-Object { $_.FullName }"', returnStdout: true).trim()
                    jarPath = jarPath.split("\n").last().trim() // Extract the actual path from the command output

                    echo "Verified JAR file path: ${jarPath}"

                    def containerName = env.JOB_NAME
                    echo "Docker container name: ${containerName}"

                    // Stop and remove existing container using Portainer API
                    bat """
                        powershell -Command "
                        \$headers = @{
                            'Authorization' = 'Bearer ${env.PORTAINER_TOKEN}'
                            'Content-Type' = 'application/json'
                        }
                        Invoke-RestMethod -Uri '${env.PORTAINER_URL}/endpoints/1/docker/containers/${containerName}?v=1' -Method DELETE -Headers \$headers -ErrorAction SilentlyContinue
                        "
                    """

                    // Run the Docker container using Portainer API
                    bat """
                        powershell -Command "
                        \$headers = @{
                            'Authorization' = 'Bearer ${env.PORTAINER_TOKEN}'
                            'Content-Type' = 'application/json'
                        }
                        \$body = @{
                            'Image' = 'dockermule'
                            'HostConfig' = @{
                                'PortBindings' = @{
                                    '8081/tcp' = @(@{
                                        'HostPort' = '8082'
                                    })
                                }
                            }
                            'Name' = '${containerName}'
                        } | ConvertTo-Json
                        \$response = Invoke-RestMethod -Uri '${env.PORTAINER_URL}/endpoints/1/docker/containers/create' -Method POST -Body \$body -Headers \$headers
                        Write-Host \$response.Id
                        "
                    """

                    // Start the Docker container using Portainer API
                    bat """
                        powershell -Command "
                        \$headers = @{
                            'Authorization' = 'Bearer ${env.PORTAINER_TOKEN}'
                        }
                        \$response = Invoke-RestMethod -Uri '${env.PORTAINER_URL}/endpoints/1/docker/containers/${containerName}/start' -Method POST -Headers \$headers
                        Write-Host \$response
                        "
                    """

                    // Copy the JAR file to the Docker container using Docker CLI
                    bat "docker cp \"${jarPath}\" ${containerName}:/opt/mule/apps"
                }
            }
        }
    }
}
