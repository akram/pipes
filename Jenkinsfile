#!/usr/bin/env groovy

import java.text.SimpleDateFormat

pipeline {
  agent none
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
        stage('initialisation') {
            steps {
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                            echo "Using project: ${openshift.project()}"
                            def bc = openshift.selector("bc", "job-1");
                            echo "BC is ${bc}"
                            openshiftVerifyBuild buildConfig: "job-1" 
                        }
                    }
                }
            }
        }
  } 
}


