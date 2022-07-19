pipeline {
  triggers {
    pollSCM ('H/3 * * * *')
  }
  options {
    disableConcurrentBuilds()
  }
  agent {
    node {
      label 'nodejs'
    }
  }
  stages {
    stage ('cleanup mytest project') {
      when {
        branch 'main'
      }
      steps {
        sh 'if oc delete project mytest 2>/dev/null; then echo "deleting mytest project"; oc wait --for=delete namespace/mytest --timeout=90s; else echo "skipping removal of mytest project"; fi'
      }
    }
    stage ('create mytest project') {
      when {
        branch 'main'
      }
      steps {
        sh 'oc new-project mytest'
      }
    }
    stage ('deploy app') {
      when {
        branch 'main'
      }
      steps {
        sh 'oc new-app --name mytest --docker-image=quay.io/ajblum/hello-openshift:latest'
        sh 'oc rollout status deployment/mytest --timeout 90s'
      }
    }
    stage ('create route') {
      when {
       branch 'main'
      }
      steps {
        sh 'oc expose service/mytest'
      }
    }
    stage ('test app') {
      when {
        branch 'main'
      }
      steps {
        sh 'curl -s -o /dev/null --fail http://mytest-mytest.apps.ocp4.example.com'
      }
    }
  }
}
