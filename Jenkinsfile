pipeline{
	agent any

	stages {
		stage("checkout cod"){
			// checkout the repository	
			steps {
				git branch: 'main', url: 'https://github.com/IvoPetrovv/JenkinsSeleniumIDE_Demo_01'
			}
		}
		stage("Set up .Net Core"){
			// install dot net
			steps {
				bat '''
				echo Downloading .Net 6 Sdk
				curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
				echo installing dotnet-sdk-6.0.132-win-x64.exe
				dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
				'''
			}
		}
		stage("Restoring nuget packages"){
			// install dependencies
			steps{
				bat 'dotnet restore SeleniumIde.sln'
			}
		}
		stage("build"){
			// build
			steps{
				bat 'dotnet msbuild "SeleniumIde.sln" -- configuration Release'
				     
			}
		}
		stage("run test"){
			// run test
			steps{
				bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResult.trx"'
				     
			}
		}
	}

	post{
		always{
			archiveArtifacts artifacts: '**/TestResult/*.trx', allowEmptyArchive: true
			step([
				$class: 'MSTestPublisher',
				testResultFile: '**/TestResult/*.trx'
			])
		}
	}
	

}
