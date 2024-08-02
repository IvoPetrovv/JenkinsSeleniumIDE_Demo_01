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
				echo Downloadind .Net 6 Sdk
				curl -l -o dotnet-sdk-6.0.132-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/ad59f1d1-5f19-4474-86be-2f09ab195618/5c7a64445dae84e386bb88e1f6ac09e4/dotnet-sdk-6.0.132-win-x86.exe
				echo installing dotnet-sdk-6.0.132-win-x86.exe
				dotnet-sdk-6.0.132-win-x86.exe /quite /norestart
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
				bat 'dotnet build SeleniumIde.sln -- configuration Release'
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
			step{[
				$class: 'MSTestPublisher',
				testResultFile: '**/TestResult/*.trx'
			]}
		}
	}
	

}