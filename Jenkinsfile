#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent { node { label 'nodejs' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
          node('maven') {
            sh '''
               oc whoami 
               '''
            }
          }
        script {
          node('nodejs') {
              sh '''
                 oc whoami
                 '''
          }
        }
      }
    }
  }
}
