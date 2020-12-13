
# Our work project
oc new-project akram-pipeline

# Create a jenkins instance
oc new-app jenkins-ephemeral


# Add some roles for the pipeline
oc adm policy add-cluster-role-to-user self-provisioner system:serviceaccount:$(oc project -q):default
oc adm policy add-role-to-user admin  system:serviceaccount:$(oc project -q):default -n $(oc project -q)

# Create a new (Jenkins) pipeline build
oc new-build  --strategy=pipeline https://github.com/akram/jenkinsfile-runner-openshift.git#f2f


# Start the build
oc start-build jenkinsfile-runner-openshift

