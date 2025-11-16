pipeline {
agent any

environment {
DEPLOY_DIR = 'C:\inetpub\wwwroot'
GIT_CREDS = 'github-khan'
}

stages {
stage('Checkout') {
steps {
echo "Cloning project from GitHub with credentialsId=${env.GIT_CREDS}"
git branch: 'master',
url: 'https://github.com/Aeju011/portfolio2.git
',
credentialsId: "${env.GIT_CREDS}"
}
}
  stage('Build (none)') {
  steps {
    echo "Static site. No build step."
  }
}

stage('Deploy to IIS (localhost)') {
  steps {
    bat '''
echo === Deploy to IIS folder ===
echo Workspace: %WORKSPACE%
echo Target: %DEPLOY_DIR%

rem Ensure target exists
if not exist "%DEPLOY_DIR%" (
echo Creating target folder %DEPLOY_DIR%
mkdir "%DEPLOY_DIR%"
if errorlevel 1 (
echo ERROR: Unable to create %DEPLOY_DIR%
exit /b 12
)
)

rem Prefer robocopy if available
where robocopy >nul 2>&1
if %ERRORLEVEL% == 0 (
echo Using robocopy to copy files
robocopy "%WORKSPACE%" "%DEPLOY_DIR%" index.html about.html /E /NFL /NDL /NJH /NJS /NP /MT:8
set RC=%ERRORLEVEL%
echo robocopy exit code %RC%
if %RC% GEQ 8 (
echo ERROR: robocopy failed with exit code %RC%
exit /b %RC%
) else (
echo robocopy completed
exit /b 0
)
) else (
echo robocopy not found, using fallback copy commands
if exist "%WORKSPACE%\index.html" (
copy /Y "%WORKSPACE%\index.html" "%DEPLOY_DIR%\index.html" >nul
if errorlevel 1 (
echo ERROR copying index.html
exit /b 13
)
) else (
echo index.html not found
)

if exist "%WORKSPACE%\about.html" (
copy /Y "%WORKSPACE%\about.html" "%DEPLOY_DIR%\about.html" >nul
if errorlevel 1 (
echo ERROR copying about.html
exit /b 14
)
) else (
echo about.html not found
)

rem copy assets folder if present using xcopy if available
where xcopy >nul 2>&1
if %ERRORLEVEL% == 0 (
if exist "%WORKSPACE%\assets" (
echo Using xcopy to copy assets folder
xcopy "%WORKSPACE%\assets" "%DEPLOY_DIR%\assets" /E /I /Y >nul
if errorlevel 1 (
echo ERROR: xcopy failed
exit /b 15
)
) else (
echo assets folder not present
)
) else (
echo xcopy not found. Skipping assets copy fallback.
)

echo Fallback copy finished
exit /b 0
)
'''
}
}
}

post {
success {
echo "Deployment finished. Open http://localhost/index.html
 on this machine."
}
failure {
echo "Deployment failed. Check Deploy stage output for errors."
}
}
}
