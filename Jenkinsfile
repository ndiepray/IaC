pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/ndiepray/IaC-jenkins.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        docker build -t greenstatic/echo-ip:1.1.0 .
        docker push greenstatic/echo-ip:1.1.0
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment pl-bulk-prod --image=greenstatic/echo-ip:1.1.0
        kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8081 \
                                               --target-port=80 --name=pl-bulk-prod-svc
        '''
      }
    }
  }
}
