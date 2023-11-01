pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            app: test
        spec:
          containers:
          - name: terraform
            image: hashicorp/terraform
            command:
            - cat
            tty: true
          - name: git
            image: bitnami/git:latest
            command:
            - cat
            tty: true
      '''
    }      
  }
  parameters {
    choice(name: 'workflow', choices: ['apply', 'destroy'], description: 'Select terraform workflow')
  }
  options {
    buildDiscarder logRotator(daysToKeepStr: '2', numToKeepStr: '10')
    timeout(time: 10, unit: 'MINUTES')
  }
  stages {
    stage('Checkout SCM') {
      // when { expression { true } }
      steps {
        container('git') {
          git credentialsId: 'github_creds', url: 'https://github.com/kunchalavikram1427/sample_terraform_code.git'
        }
      }
    }
    stage('Terraform Init'){
      // when { expression { true } }
      steps {
        container('terraform'){
          withCredentials([usernamePassword(credentialsId: 'aws_creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            sh '''
              terraform init
            '''
          }
        }
      }
    }
    stage('Terraform Apply'){
      when { expression { true } }
      steps {
        container('terraform'){
          withCredentials([usernamePassword(credentialsId: 'aws_creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
            sh '''
              terraform ${workflow} -auto-approve
            '''
          }
        }
      }
    }
  }
}       
