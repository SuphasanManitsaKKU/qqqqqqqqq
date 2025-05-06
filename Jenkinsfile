pipeline {
    agent any

    tools {
        nodejs 'node24'
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
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

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
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: false
            }
        }

        stage('Deploy to Host') {
            steps {
                sh '''
            DEPLOY_DIR=/mnt/deploy-outside
            echo "🛠️ Ensuring deployment directory exists..."
            mkdir -p $DEPLOY_DIR
            echo "🚮 Cleaning up old deployment..."
            rm -rf $DEPLOY_DIR/dist

            echo "📦 Copying new build to $DEPLOY_DIR..."
            cp -r dist/ $DEPLOY_DIR/dist

            echo "🚀 Signaling host to restart app..."
            chmod +x $DEPLOY_DIR/restart.sh
            $DEPLOY_DIR/restart.sh
        '''
            }
        }
    }
}