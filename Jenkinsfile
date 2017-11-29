import groovy.transform.Field

def branchName = env.BRANCH_NAME
def gitURL = scm.getUserRemoteConfigs()[0].getUrl()

node('windows-slave') {

  //Set when to trigger scan for changes on repo
   triggers { 
     pollSCM('H/5 * * * *') //Set reasonable update time here. We will investigate how to use server side git hook instead of polling
   }

   try {        
        stage('Checkout') {
         deleteDir()
         checkout scm
        }

        stage('Setup') {
          //initial setup, manage dependencies etc
        }

        stage('Linting') {
          //executing liting here
        }

        stage('Build') {
          //call to gradle to build flavor here
        }

        stage('Unit Tests') {
          //trigger unit test here
          //publish unit test report to jenkins using 
           //junit 'test_output/report.junit'
        }


        stage('Upload') {
          //use gradle or nexus plugin to upload to TF here
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
    //example send email/slack notification etc here
    slackSend channel: '#ios', color: 'danger', message: ":dizzy_face: Build failed ${env.JOB_NAME} (${env.BUILD_NUMBER})\n${env.BUILD_URL}"
    echo 'Stage Failed!!!'
    
    deleteDir()
    throw e
  }
}