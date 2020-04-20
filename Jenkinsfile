#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent { kubernetes { cloud 'openshift' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
         container("maven") {
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


