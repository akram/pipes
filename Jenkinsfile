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
            git url: 'https://github.com/akram/pipes.git'
            sh '''
               cd pipes
               mvn clean package -X
               '''
            }
          }
        script {
          node('nodejs') {
            openshift.withCluster() {
              def currentProject
              openshift.withProject() {
                currentProject = openshift.project()
                def project = "test-" + new SimpleDateFormat("yyyy-MM-dd-HHmmss").format(new Date())
                echo "To allow jenkins to create projects from a pipeline, the following command must be run"
                echo "oc adm policy add-cluster-role-to-user self-provisioner system:serviceaccount:$currentProject:jenkins"
                openshift.raw( "new-project $project" )
                // echo "Context project is $openshift.project()"
                // Project context has been set to the pipeline project
  	      currentProject = project
                echo "openshift.raw() commands will specify $currentProject as project"
                // but then we will forcibely specify the new-project as the project to run raw commands against
                echo "start"
                def result = ""
                echo "\"adm prune deployments\", \" --keep-complete=0 --keep-failed=0 -n ${currentProject}\""
                result = openshift.raw( "adm prune deployments", "--namespace=${currentProject} --keep-complete=0 --keep-failed=0 -n ${currentProject}")
                echo "${result}"

                echo "\"adm prune deployments\", \"-n ${currentProject}\", \" --keep-complete=0 --keep-failed=0 \""
                result = openshift.raw( "adm prune deployments", "-n ${currentProject}", "--keep-complete=0 --keep-failed=0 -n ${currentProject}")
                echo "${result}"

                echo "\"adm prune deployments\", \" --keep-complete=0 --keep-failed=0\", \"-n ${currentProject}\""
                result = openshift.raw( "adm prune deployments", " --keep-complete=0 --keep-failed=0", "-n ${currentProject}")
  	      echo "${result}"

                echo "\"adm prune deployments  --keep-complete=0 --keep-failed=0 -n ${currentProject}\""
                result = openshift.raw( "adm prune deployments  --keep-complete=0 --keep-failed=0 -n ${currentProject}")
                echo "${result}"

                echo "\"adm -n ${currentProject} prune deployments  --keep-complete=0 --keep-failed=0 \""
                result = openshift.raw( "adm -n ${currentProject} prune deployments  --keep-complete=0 --keep-failed=0")
                echo "${result}"

  	      echo "\"adm --namespace=${currentProject} prune deployments  --keep-complete=0 --keep-failed=0 \""
                result = openshift.raw( "adm --namespace=${currentProject} prune deployments  --keep-complete=0 --keep-failed=0")
                echo "${result}"

                echo "end"
              }
            }
          }
        }
      }
    }
  }
}
