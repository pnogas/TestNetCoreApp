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
                    bat "dotnet \"C:\\Program Files\\sonar-scanner\\SonarScanner.MSBuild.dll\" begin /k:TestNetCoreApp"
                    bat "dotnet build TestNetCoreApp.sln"
                    bat "dotnet \"C:\\Program Files\\sonar-scanner\\SonarScanner.MSBuild.dll\" end"
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