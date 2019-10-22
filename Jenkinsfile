#!/usr/bin/env groovy
pipeline {
  agent { node { label 'nodejs' } }
  options { timeout(time: 20, unit: 'MINUTES') }
  stages {
    stage('raw') {
      steps {
        script {
          openshift.withCluster() {
            openshift.withProject() {
              def result
              result = openshift.raw( "adm prune deployments", "--keep-younger-than=168h --keep-complete=0 --keep-failed=0 -n test-project")
              echo $result
              result = openshift.raw( "adm prune deployments", "--keep-younger-than=168h --keep-complete=0 --keep-failed=0", "-n test-project")
              echo $result
              result = openshift.raw( "adm prune deployments --keep-younger-than=168h --keep-complete=0 --keep-failed=0 -n test-project")
              echo $result

            }
          }
        }
      }
    }
  }
}


