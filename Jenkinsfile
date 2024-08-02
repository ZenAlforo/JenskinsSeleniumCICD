pipeline {
    agent any

    stages { 
        stage("Checkout Code") {
            // checkout the repository
            steps {
                git branch: "main", url: 'https://github.com/ZenAlforo/JenskinsSeleniumCICD'
            }
        }

        stage("Set up .Net Core") {
            // checkout the repository
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
                echo Installing dotnet-sdk-6.0.132-win-x64.exe
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
            }
        }

        stage("Restoring nuget packages") {
            // checkout the repository
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }       

        stage("Build") {
            // checkout the repository
            steps {
                bat 'dotnet restore SeleniumIde.sln --configuration Release'
            }
        }  

        stage("Run Tests") {
            // checkout the repository
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx'
            }
        }  
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultFile: '**/TestResults/*.trx'
            ])
        }
    }
}