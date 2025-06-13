pipeline {
 agent any
 
 stages {
	stage('clone'){
		steps {
			echo 'Cloning source code'
			git branch:'main', url: 'https://github.com/huudqtmu/NETCORE2.git'
		}
	} // end clone

	stage('restore package') {
		steps
		{
			echo 'Restore package'
			bat 'dotnet restore'
		}
	}

	stage ('build') {
		steps {
			echo 'build project netcore'
			bat 'dotnet build  --configuration Release'
		}
	}

	stage ('tests') {
		steps{
			echo 'running test...'
			bat 'dotnet test --no-build --verbosity normal'
		}
	}

	stage ('public den t thu muc')
	{
		steps{
			echo 'Publishing...'
			bat 'dotnet publish -c Release -o ./publish'
		}
	}

	stage ('Publish') {
		steps {
			echo 'public 2 runnig folder'
			bat 'iisreset /stop'
			bat 'xcopy "%WORKSPACE%\\publish" /E /Y /I /R "c:\\wwwroot\\netcore2"'
			bat 'iisreset /start'

 		}
	}

	stage('Deploy to IIS') {
            steps {
                powershell '''
               
                # Tạo website nếu chưa có
                Import-Module WebAdministration
                if (-not (Test-Path IIS:\\Sites\\netcore2)) {
                    New-Website -Name "netcore2" -Port 85 -PhysicalPath "c:\\wwwroot\\netcore2"
                }
                '''
            }
        } // end deploy iis

  } // end stages
}//end pipeline
