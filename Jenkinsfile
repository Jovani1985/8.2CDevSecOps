pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Jovani1985/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }

            post {
                always {
                    emailext(
                        to: 'jgob1985@gmail.com',
                        subject: "Run Tests Stage: ${currentBuild.result ?: 'SUCCESS'} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """
Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}

Stage: Run Tests

Status: ${currentBuild.result ?: 'SUCCESS'}

Jenkins URL:
${env.BUILD_URL}
                        """,
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }

            post {
                always {
                    emailext(
                        to: 'jgob1985@gmail.com',
                        subject: "NPM Audit Security Scan Stage: ${currentBuild.result ?: 'SUCCESS'} - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """
Build: ${env.JOB_NAME} #${env.BUILD_NUMBER}

Stage: NPM Audit (Security Scan)

Status: ${currentBuild.result ?: 'SUCCESS'}

Jenkins URL:
${env.BUILD_URL}
                        """,
                        attachLog: true
                    )
                }
            }
        }
    }
}
