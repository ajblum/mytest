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
    stage ('create project') {
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
        sh 'curl -s --fail http://mytest-mytest.apps.ocp4.example.com'
      }
    }
  }
}
