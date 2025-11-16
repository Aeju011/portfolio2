pipeline {
agent any

environment {
DEPLOY_DIR = 'C:\inetpub\wwwroot'
GIT_CREDS = 'github-khan'
}

stages {
stage('Checkout') {
steps {
echo "Cloning from GitHub with credentialsId=${env.GIT_CREDS}"
git branch: 'master',
url: 'https://github.com/Aeju011/portfolio2.git
',
credentialsId: "${env.GIT_CREDS}"
}
}
  echo === Deploy to IIS folder ===
echo Workspace: %WORKSPACE%
echo Target: %DEPLOY_DIR%

if not exist "%DEPLOY_DIR%" (
echo Creating target folder %DEPLOY_DIR%
mkdir "%DEPLOY_DIR%"
)

where robocopy >nul 2>&1
if %ERRORLEVEL% == 0 (
echo Using robocopy
robocopy "%WORKSPACE%" "%DEPLOY_DIR%" index.html about.html /NFL /NDL /NJH /NJS /NP /MT:8
exit /b 0
)

echo robocopy not found, using copy
copy /Y "%WORKSPACE%\index.html" "%DEPLOY_DIR%\index.html"
copy /Y "%WORKSPACE%\about.html" "%DEPLOY_DIR%\about.html"

exit /b 0
'''
}
}
}

post {
success {
echo "Deployment finished. Open http://localhost/index.html
"
}
failure {
echo "Deployment failed. Read Deploy stage logs."
}
}
}
