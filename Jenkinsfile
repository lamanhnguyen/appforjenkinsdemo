pipeline {
  agent any
  environment {
    APPSYSID = '5e0e65cd87e18a10271233383cbb3564'
    PUBLISHEDAPPVERSION = '1.0.6'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = 'sn-creds'
    DEVENV = 'https://democn5.service-now.com/'
    TESTENV = 'https://democn3.service-now.com/'
    PRODENV = 'https://democn.service-now.com/'
    TESTSUITEID = '845f8b900b20220050192f15d6673aee'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}", appVersionToInstall: "${PUBLISHEDAPPVERSION}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
