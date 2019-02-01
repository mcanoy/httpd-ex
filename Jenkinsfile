pipeline {
  agent { label 'jenkins-slave-ansible' }
    
  environment {
    CI_CD_PROJECT = "${OPENSHIFT_BUILD_NAMESPACE}"
    SOURCE_CONTEXT_DIR = "."
    APP_NAME = "httpd-sample"
    OCP_API_SERVER = "${OPENSHIFT_API_URL}"
    OCP_TOKEN = readFile('/var/run/secrets/kubernetes.io/serviceaccount/token').trim()

  }

  stages {

    stage('Container Build'){
      steps {
        patchBuildConfigOutputLabels(bcName: "${APP_NAME}")
        script {
          openshift.withCluster () {
            openshift.startBuild( "${env.APP_NAME} --from-dir=${env.SOURCE_CONTEXT_DIR} -w" )
          }
        }
      }
    }

    stage('Deploy: Dev'){
      steps {
        script{
          DEV_PROJECT = "${OPENSHIFT_BUILD_NAMESPACE}".reverse().drop(6).reverse()  + "-dev"
//          applyAnsibleInventory( 'dev' )
//          timeout(5) { // in minutes
//            promoteImageWithinCluster( "${CI_CD_PROJECT}/${APP_NAME}:latest",     "${DEV_PROJECT}/${APP_NAME}", "deployed" )
//            verifyDeployment("${APP_NAME}", "${DEV_PROJECT}")
 //         }
        }
      }
    }

    stage('Deploy: Demo'){
      steps {
        script{
          DEMO_PROJECT = "${OPENSHIFT_BUILD_NAMESPACE}".reverse().drop(6).reverse()  + "-demo"
//          applyAnsibleInventory( 'demo' )
//          timeout(5) { // in minutes
//            promoteImageWithinCluster( "${CI_CD_PROJECT}/${APP_NAME}:latest",     "${DEMO_PROJECT}/${APP_NAME}", "deployed" )
//            verifyDeployment("${APP_NAME}", "${DEMO_PROJECT}")
//          }
        }
      }
    }
  }

  post {
    always {
      echo "Finished"
    }
  } 

}



