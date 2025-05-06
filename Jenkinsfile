pipeline {
    agent any

    tools {
        nodejs "node24" // Node.js version to use 24
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        sh """
                            node -v
                            ${scannerHome}/bin/sonar-scanner
                        """
                    }
                }
            }
        }
    }
}