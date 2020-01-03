#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent { node { label 'nodejs' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
         node("maven") {
          openshift.withCluster() {
            openshift.withProject() {
              def currentProject = openshift.project()
              def project = "test-" + new SimpleDateFormat("yyyy-MM-dd-HHmmss").format(new Date()) 
              def templateSelector = openshift.selector( "template", "nodejs-example")
              if (!templateSelector.exists() ) {
                  openshift.create("https://raw.githubusercontent.com/openshift/nodejs-ex/master/openshift/templates/nodejs.json")
              } 
              openshift.newApp("nodejs-example", "--template=${currentProject}/nodejs-example")
              openshift.patch("dc/nodejs-example", '\'{"spec":{"strategy":{"type":"Recreate"}}}\'')
              def mySelector = openshift.selector("bc/nodejs-example")
              mySelector.patch('\'{"spec":{"source":{"git":{"ref": "development"}}}}\'')
              echo "end"            
           }
          }
         }
        }
      }
    }
    stage('nodejs') {
      steps {
        script {
         node("nodejs") {
          openshift.withCluster() {
            openshift.withProject() {
              def currentProject = openshift.project()
              def project = "test-" + new SimpleDateFormat("yyyy-MM-dd-HHmmss").format(new Date())
              echo "To make this pipeline work it is required to create a secret named my-private-ssh-key and make it sync"
              echo "oc create secret generic my-private-ssh-key --from-file=ssh-privatekey=$HOME/.ssh/id_rsa --from-literal=username=akram"
              echo "oc label secret my-private-ssh-key  credential.sync.jenkins.openshift.io=true"
              echo "This is a test $project "
              def credentialsId = "${currentProject}-my-private-ssh-key"
              git branch: 'master', url: 'https://github.com/akram/pipes.git'
              echo "end"
           }
          }
         }
        }
      }
    }


  }
}


