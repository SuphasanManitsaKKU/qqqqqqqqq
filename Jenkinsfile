pipeline {
    agent any

    tools {
        nodejs "node24"
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        sh """
                            ${scannerHome}/bin/sonar-scanner
                        """
                    }
                }
            }
        }

        // stage('Run Tests and Generate Coverage') {
        //     steps {
        //         sh 'npm run test'
        //     }
        // }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // archiveArtifacts artifacts: 'coverage/**, dist/**', allowEmptyArchive: false
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: false
            }
        }
    }

    // post {
    //     always {
    //         junit 'coverage/clover.xml'
    //     }
    // }
}