import groovy.transform.Field

def branchName = env.BRANCH_NAME
def gitURL = scm.getUserRemoteConfigs()[0].getUrl()

node('windows-slave') {

  //Set when to trigger scan for changes on repo
   triggers { 
     pollSCM('H/5 * * * *') 
   }

   try {        
        stage('Checkout') {
         deleteDir()
         checkout scm
        }

        stage('Setup') {
          //unlock keychain etc
          //carthage dependencies
        }

        stage('Linting') {
          sh 'fastlane lint' //using swiftlint
        }

        stage('Build') {

        }

        stage('Unit Tests') {
          sh 'fastlane test'
        }


        stage('Upload') {

          sh 'fastlane deploy_nexus'
          sh 'fastlane deploy_testfairy' //use testers_groups and notify parameter to notify right testers group for boon pöanet
        }

        stage('SeeTest') {
            //Here we will in the future hook up automated seetest ui test
        }

        stage('Post') {
          echo 'delete workspace here' 
          //We have to delete ws here, otherwise we will have one folder with repo checked out for every new branch we build
          deleteDir()

        }

  } catch (e) {
    slackSend channel: '#ios', color: 'danger', message: ":dizzy_face: Build failed ${env.JOB_NAME} (${env.BUILD_NUMBER})\n${env.BUILD_URL}"
    echo 'Stage Failed!!!'
    
    deleteDir()
    throw e
  }
}