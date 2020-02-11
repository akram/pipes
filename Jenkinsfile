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
                            def bc = openshift.selector("bc", "pipes");
                            echo "BC is ${bc}"
                            def now = new SimpleDateFormat("yyyy-MM-dd-HHmmss").format(new Date()) 
                            def app = openshift.newApp('redis-ephemeral',"-p", "DATABASE_SERVICE_NAME=redis-${now}").narrow('dc')
                            def dc = app.object()
                            echo "new-app created a ${dc.kind} with name ${dc.metadata.name}"
                            echo "The object has labels: ${dc.metadata.labels}"
                            openshiftVerifyBuild buildConfig: "job-1" 
                        }
                    }
                }
            }
        }
  } 
}


