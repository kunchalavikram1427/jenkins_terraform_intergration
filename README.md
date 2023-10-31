# Terraform with Jenkins 

## Youtube Video URL
```
https://
```
## Install Jenkins
https://www.jenkins.io/doc/book/installing/kubernetes/#install-jenkins-with-helm-v3
```
helm repo add jenkinsci https://charts.jenkins.io
helm repo update
helm search repo
helm pull jenkinsci/jenkins --untar
```
Run 
```
helm upgrade --install jenkins jenkins/
```
or install without making any changes to the chart by running
```
helm upgrade --install jenkins jenkinsci/jenkins --set controller.servicePort=80 --set controller.serviceType=LoadBalancer
```
Get password
```
kubectl exec -it jenkins-0 -- bash
cat /run/secrets/additional/chart-admin-password
```
## Pipeline Steps Snippets

### Agent Template
https://plugins.jenkins.io/kubernetes/
```
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
```
### Build Options
https://www.jenkins.io/doc/book/pipeline/syntax/#options
```
options {
  buildDiscarder logRotator(daysToKeepStr: '2', numToKeepStr: '10')
  timeout(time: 10, unit: 'MINUTES')
}
```

## Plugins to use
```
https://plugins.jenkins.io/github/
https://plugins.jenkins.io/embeddable-build-status/
https://plugins.jenkins.io/matrix-auth/
```

## Links
```
https://hub.docker.com/r/hashicorp/terraform
https://registry.terraform.io/providers/hashicorp/aws/latest/docs
https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#handling-credentials
https://www.jenkins.io/doc/book/pipeline/syntax/#input
https://www.jenkins.io/doc/book/pipeline/syntax/#parameters
https://www.jenkins.io/doc/book/pipeline/syntax/#options
```

## Author
- Vikram K (www.youtube.com/c/devopsmadeeasy)
