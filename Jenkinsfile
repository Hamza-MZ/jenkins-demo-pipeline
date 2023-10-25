pipeline {
  agent any
  environment {
    PATH = "C:\\Windows\\System32;%PATH%"
  }

  options {
    buildDiscarder logRotator(
      daysToKeepStr: '16',
      numToKeepStr: '10'
    )
  }

  stages {
    stage('Setup Environment for APICTL') {
      steps {
        bat '''
          SETLOCAL EnableDelayedExpansion
          FOR /F %%A IN ('apictl list envs --format {{.}} ^| find /c /v ""') DO SET ENVCOUNT=%%A
          IF !ENVCOUNT! EQU 0 (
            apictl add-env -e live --apim https://localhost:9447
          )
          ENDLOCAL
        '''
      }
    }

    stage('Deploy APIs To "Live" Environment') {
      steps {
        bat '''
          apictl login live -u admin -p admin
          apictl vcs deploy -e live
        '''
      }
    }
  }
}