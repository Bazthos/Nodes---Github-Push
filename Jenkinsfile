pipeline {
    agent any
    
    environment {
        // Définir les variables d'environnement si nécessaire
        GIT_URL = 'https://github.com/Bazthos/Nodes---Github-Push.git'
        PROJECT_DIR = 'Nodes---Github-Push'
    }

    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    // Vérifier si le dossier existe et le nettoyer
                    if (isUnix()) {
                        sh """
                        if [ -d "${PROJECT_DIR}" ]; then
                            echo "Cleaning existing directory"
                            rm -rf ${PROJECT_DIR}
                        fi
                        """
                    } else {
                        powershell """
                        if (Test-Path -Path "${PROJECT_DIR}") {
                            Write-Host "Cleaning existing directory"
                            Remove-Item "${PROJECT_DIR}" -Force -Recurse
                        }
                        """
                    }
                }
            }
        }

        
        stage('Install Dependencies') {
            steps {
                script {
                    dir("${PROJECT_DIR}") {
                        // Exécuter npm install dans le répertoire du projet cloné
                        powershell 'npm install'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    dir("${PROJECT_DIR}") {
                        // Exécuter npm test dans le répertoire du projet cloné
                        powershell 'npm test'
                    }
                }
            }
        }
    }
    // autres configurations
    
    post {
        always {
            script {
                dir("${PROJECT_DIR}/test_results") {
                    junit 'junit.xml'
                }
            }
            echo 'Sending email notification...'
            // Configurer pour envoyer des emails ici
        }
    }
}