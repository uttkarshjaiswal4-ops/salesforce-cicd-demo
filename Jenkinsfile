pipeline {
    agent any
    
    environment {
        SF_CLI_PATH = 'C:\\Users\\Uttkarsh.Jaiswal\\AppData\\Roaming\\npm\\sf.cmd'
        SF_TARGET_ORG = credentials('SALESFORCE_TARGET_ORG')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/uttkarshjaiswal4-ops/salesforce-cicd-demo.git'
            }
        }
        
        stage('Authorize Target Org') {
            steps {
                script {
                    bat """
                        echo %SF_TARGET_ORG% > authFile.txt
                        "%SF_CLI_PATH%" org login sfdx-url --sfdx-url-file authFile.txt --alias targetOrg
                        del authFile.txt
                    """
                }
            }
        }
        
        stage('Deploy to Target Org') {
            steps {
                bat "\"%SF_CLI_PATH%\" project deploy start --target-org targetOrg --source-dir force-app --test-level NoTestRun"
            }
        }
    }
    
    post {
        always {
            echo 'Deployment completed!'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed! Check logs above.'
        }
    }
}