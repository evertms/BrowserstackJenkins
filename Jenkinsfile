pipeline {
    agent any

    environment {
        ALLURE_RESULTS = "${env.WORKSPACE}\\allure-results"
        ALLURE_REPORT = "${env.WORKSPACE}\\allure-report"

        BROWSERSTACK_USERNAME = credentials('browserstack-username')
        BROWSERSTACK_ACCESS_KEY = credentials('browserstack-access-key')
        
        APP_PATH='bs://fe0b7f3f0f3d51e7c52767bb80e6e60ef87d1677'
    }

    stages {

        stage('Clean') {
            steps {
                echo 'Limpiando workspace'
                deleteDir()
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/evertms/BrowserstackJenkins.git'
            }
        }

        stage('Build') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npx wdio wdio.browserstack.conf.js --cucumberOpts.tags="@Smoke"'
            } 
        }

        stage('Report') {
            steps {
                echo "Generando reporte"
                bat "npx allure generate %ALLURE_RESULTS% -c -o %ALLURE_REPORT%"
            }
        }

        stage('Publish report') {
            steps {
                echo "Publicando reporte"
                allure includeProperties: 
                    false,
                    jdk: '',
                    results: [[path: 'allure-results']] 
            }
        }
    }

    post {
        always {
            echo "Pipeline finalizada"
        }
    }
}