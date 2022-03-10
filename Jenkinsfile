pipeline {
    agent any
    environment {
      // Configure in global tool setting of jenkins slave or master
        SLACK_UPLOAD_FILE_TOKEN = "dev-slack-app-token"
        CMDLINE = "clean initialize package"
    }
    stages {
      stage('Cleanup')
      {
        steps {
          bat 'set'
          echo "Cleaning it up..."
          echo currentBuild.projectName
        }
      }
      stage('Compile & Build') 
      {
        steps {
          echo "Compile & Build..."
          bat 'mvn -f Corp_Emp_RR_AppianWorkdayREST.application.parent/pom.xml clean initialize package %TIBCO_MVN_OPTIONS%'
        }
      }
      stage('Deploy') 
      {
        steps {
          echo "Deploying..."
          bat 'mvn -f Corp_Emp_RR_AppianWorkdayREST.application.parent/pom.xml install %TIBCO_MVN_OPTIONS%'
        }
      }
      stage('QA Test') 
      {
        steps {
          echo "QA Test..."
        }
      }
    }
    post {
      success {
        archiveArtifacts artifacts: '*.application/target/*.ear'
        bitbucketStatusNotify(buildState: "SUCCESSFUL")
      }
      failure {
        bitbucketStatusNotify(buildState: "FAILED")
      }
    }
    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '25', artifactNumToKeepStr: '25'))
    }
}
