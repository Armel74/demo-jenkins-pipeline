pipeline {
    agent {
        // Utilisation d'une image Docker avec Maven et JDK préinstallés
        docker {
            image 'maven:3.9.6-eclipse-temurin-17'
            args '-v /root/.m2:/root/.m2'  // Cache Maven local pour accélérer les builds
        }
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Clonage du dépôt GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '⚙️ Compilation du projet...'
                sh 'mvn -B clean compile'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Exécution des tests unitaires...'
                sh 'mvn -B test'
            }
            post {
                always {
                    // Publier les résultats de tests JUnit dans Jenkins
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline ! Vérifie les logs et les tests.'
        }
        unstable {
            echo '⚠️ Pipeline instable (tests en échec partiel ou avertissements).'
        }
        always {
            echo '📋 Fin de l’exécution du pipeline.'
        }
    }
}