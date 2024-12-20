pipeline {
    agent any
    environment {
        MAVEN_HOME = tool 'Maven'
        SONAR_PROJECT_KEY = 'GestionBibliotheque'
        SONAR_SCANNER_HOME=tool 'SonarScanner'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Naciiim/GestionBibliotheque.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                bat '%MAVEN_HOME%/bin/mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                bat '%MAVEN_HOME%/bin/mvn test'
            }
        }
        stage('Quality Analysis') {
            steps {
            echo 'Starting Quality Analysis...'
            withCredentials([string(credentialsId: 'SonarQube-GestionBibliotheque-token', variable: 'SONAR_TOKEN')])
              {  withSonarQubeEnv('SonarQube') {
                    bat """
                     echo Running SonarQube analysis
                              mvn sonar:sonar ^
                               -Dsonar.host.url=http://localhost:9000 ^
                               -Dsonar.login=%SONAR_TOKEN%
                               -X
                               """
                     }      }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement simulé réussi'
            }
        }
    }
    post {
        success {
            emailext to: 'nassim2.9mita@gmail.com',
                subject: 'Build Success',
                body: 'Le build a été complété avec succès.'
        }
        failure {
            emailext to: 'nassim2.9mita@gmail.com',
                subject: 'Build Failed',
                body: 'Le build a échoué.'
        }
    }
}
