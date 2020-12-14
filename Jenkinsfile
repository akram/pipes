#!/usr/bin/env groovy
import java.text.SimpleDateFormat
pipeline { 
  agent none; 
  stages {
    stage('raw') {
      steps { 
        node('nodejs') {
          sh """
            git clone https://github.com/akram/simple-nodejs-ex.git
            cd simple-nodejs-ex
            npm install
            """
        }
      }
    }
    stage('raw2') {
      steps { 
        node('nodejs') {
          openshift.withCluster() {
            try {
              def created = openshift.newApp( 'nodejs~https://github.com/akram/simple-nodejs-ex.git' )
                echo "new-app created ${created.count()} objects named: ${created.names()}"
            } catch ( e ) {
              "Error encountered: ${e}"
            }
            //openshift.raw( "new-app nodejs~https://github.com/akram/simple-nodejs-ex.git " )
            def bc = openshift.selector( "bc", "simple-nodejs-ex");
            def buildSelector = bc.startBuild("--wait");
            openshift.withProject() {
              currentProject = openshift.project();
              def project = "test-" + new SimpleDateFormat("yyyy-MM-dd-HHmmss").format(new Date());
              echo "To allow jenkins to create projects from a pipeline, the following command must be run";
              echo "oc adm policy add-cluster-role-to-user self-provisioner system:serviceaccount:$currentProject:jenkins";
              echo "openshift.raw() commands will specify $currentProject as project";
              echo "end";
            }
          }
        }
      }
    }

    stage('raw3') {
      steps { 
        node('maven') {
          //git url: 'https://github.com/akram/simple-java-ex.git'
          sh """
            git clone https://github.com/akram/simple-java-ex.git
            cd simple-java-ex
            mvn -B test
            """
        }
      }
    }       
    stage('raw4') {
      steps { 
        node('maven') {
          openshift.withCluster() {
            try {
              openshift.raw( "new-app --build-env=MAVEN_ARGS_APPEND=-Dcom.redhat.xpaas.repo.jbossorg jboss-eap73-openshift:7.3~https://github.com/akram/simple-java-ex.git " );
              def created = openshift.newApp( '--build-env=MAVEN_ARGS_APPEND=-Dcom.redhat.xpaas.repo.jbossorg jboss-eap73-openshift:7.3~https://github.com/akram/simple-java-ex.git' );
              echo "new-app created ${created.count()} objects named: ${created.names()}";
            } catch ( e ) {
              echo "Error encountered: ${e}";
            }
            //openshift.raw( "new-app nodejs~https://github.com/akram/simple-nodejs-ex.git " )
            //openshift.raw( "new-app --build-env=MAVEN_ARGS_APPEND=-Dcom.redhat.xpaas.repo.jbossorg jboss-eap73-openshift:7.3~https://github.com/akram/simple-java-ex.git " )
          }
        }
      }
      stage('last') { 
        echo "Building OpenShift container image example;";
        steps { 
          openshift.withCluster() {
            openshift.withProject() {
              openshift.selector("bc", "simple-java-ex").startBuild("--follow=true");
            }
          }
        }
      }
    }
  }
}

