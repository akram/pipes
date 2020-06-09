#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent { node { label 'maven' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
          node('maven') {
            git url: 'https://github.com/akram/pipes.git'
            sh '''
               git clone https://github.com/akram/pipes.git
               cd pipes
               mvn clean package
               '''
            }
        }
      }
    }
  }
}
