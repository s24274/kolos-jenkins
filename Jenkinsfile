pipeline {
    agent any

    environment {
        NODE_VERSION = '14.x'
    }

    stages {
        stage('Checkout') {
            steps {
                // pobieranie kodu
                git 'https://github.com/s24274/kolos-jenkins.git'
            }
        }
        
        stage('Install Node.js') {
            steps {
                // instaluje Node.js i npm
                sh 'curl -sL https://deb.nodesource.com/setup_${NODE_VERSION} | bash -'
                sh 'apt-get install -y nodejs'
            }
        }

        stage('Install Dependencies') {
            steps {
                // instaluje zależności
                sh 'npm install'
            }
        }

        stage('Lint') {
            steps {
                // analizę statyczna kodu za pomocą ESLint
                sh 'npx eslint .'
            }
        }

        stage('Build') {
            steps {
                // Skompiluj aplikację React
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // uruchamia testy jednostkowe z raportowaniem
                sh 'npm test -- --coverage'
            }
            post {
                always {
                    // archiwizuje wyniki testów
                    junit '**/test-results/*.xml'
                    // archiwizuje raport pokrycia kodu
                    archiveArtifacts '**/coverage/**'
                }
            }
        }
    }

    post {
        always {
            // czyszczenie środowiska
            cleanWs()
        }
    }
}
