pipeline {
    agent any

    stages {
        
        stage('build and install') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.51.0-noble'
                }
            }

            steps {
                script {
                    sh 'npm ci'
                    //sh 'npx cucumber-js --format json:reports/cucumber-report.json'
                    sh 'npx cucumber-js'
                    stash name: 'allure-results', includes: 'allure-results/*'
                }
            }

        }
    }

    post {
        always {
            unstash 'allure-results' //extract results
            script {
                allure([
                includeProperties: false,
                jdk: '',
                properties: [],
                reportBuildPolicy: 'ALWAYS',
                results: [[path: 'allure-results']]
            ])
            }
        }
    }
}