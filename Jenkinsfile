// Sample Jenkinsfile using ssh and a zDT Agent (Nlopez)
// for help: https://www.jenkins.io/doc/book/pipeline/jenkinsfile/
// tried build on wazi and cd on zDT - need new pack/pub script --- defer. Stcik with zDT env
def myApp       = 'poc-app'
scripts         = '/u/nlopez/tmp/dbb-zappbuild/scripts'
//scripts         = '/u/nlopez/waziDBB/dbb-zappbuild/scripts'

pipeline {
    agent  { label 'myZOS-Agent' }
    //agent  { label 'myWazi-agent' }
    options { skipDefaultCheckout(true) }

    stages {
        stage('Clone') {
            steps {
                println '** Cloning on USS ...'                             
                //sh 'rm -r ' + env.WORKSPACE+'/* >/dev/null 2>&1 ; ' + scripts+'/CI/Clone2.sh ' +  env.WORKSPACE + ' ' +  myApp + ' git@github.com:nlopez1-ibm/poc-workspace.git ' + env.BRANCH_NAME 
                sh scripts+'/CI/Clone2.sh ' +  env.WORKSPACE + '  poc-workspace  git@github.com:nlopez1-ibm/poc-workspace.git ' + env.BRANCH_NAME 
            }          
        }  

        stage('Build') {
            steps {
                println  '** Building feature with DBB ...'                  
                sh scripts+'/CI/Build.sh ' + env.WORKSPACE + ' poc-workspace  ' + myApp + ' --userBuild poc-app/cobol/datbatch.cbl'
            }
        }             


        stage('Package') {
            steps {
                println  '** Packaging artifacts ...'
                sh scripts+'/CI/Package_Create.sh ' +  env.WORKSPACE + ' poc-workspace  ' + ' poc-app ' +  env.BUILD_ID 
            }
        }                

        //stage('Publish') {
        //    steps {
        //        println  '** Packaging artifacts and Publishing to UCD Code Station ...'
        //        sh scripts+'/CD/UCD_Pub.sh ' + env.BUILD_ID + ' poc-app ' + env.WORKSPACE+'/poc-workspace  '
        //    }
        //}                
    }    
}
