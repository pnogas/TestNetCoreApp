pipeline {
    agent any
    stages {
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: env.BRANCH_NAME]],
                    userRemoteConfigs: [[url: 'https://github.com/pnogas/TestNetCoreApp.git']]
                ])
            }
        }
        stage('Start Code Analysis') {
            steps {
                withSonarQubeEnv('PaulSonarQube') {
                    bat "SonarScanner.MSBuild.exe  begin /k:TestNetCoreApp"
                    bat "dotnet build TestNetCoreApp.sln"
                    bat "SonarScanner.MSBuild.exe end"
                }
            }
        }
        stage("Await Code Analysis Result") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}