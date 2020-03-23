#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent { node { label 'ansible' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
          openshift.withCluster() {
            def currentProject
            openshift.withProject() {
              sh '''
                 ansible-playbook
              '''
            }
          }
        }
      }
    }
  }
}

