pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${env.BRANCH_NAME}"
                git branch: env.BRANCH_NAME, url: 'https://github.com/shaikhmxizz/sadhix.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add your build commands here if needed
                sh 'echo "Simulating build..."'
            }
        }

        stage('Deploy to UAT') {
            when {
                branch 'uat' // This stage runs only for the UAT branch
            }
            steps {
                echo 'Deploying to UAT Nginx server...'
                publishOverSshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'UAT', // Your configured SSH server name
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '/*', // All files in the workspace
                                    removePrefix: '', // No prefix removed
                                    remoteDirectory: '/usr/share/nginx/html', // Nginx web root on UAT server
                                    execCommand: 'sudo systemctl restart nginx' // Restart Nginx to apply changes
                                )
                            ]
                        )
                    ]
                )
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'prod' // This stage runs only for the Prod branch
            }
            steps {
                echo 'Deploying to Prod Nginx server...'
                publishOverSshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'PROD', // Your configured SSH server name
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '/*', // All files in the workspace
                                    removePrefix: '',
                                    remoteDirectory: '/usr/share/nginx/html', // Nginx web root on Prod server
                                    execCommand: 'sudo systemctl restart nginx' // Restart Nginx to apply changes
                                )
                            ]
                        )
                    ]
                )
            }
        }
    }
}
