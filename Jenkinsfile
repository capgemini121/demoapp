pipeline {
  agent {
    kubernetes {
           //cloud 'kubernetes'
      yaml """
kind: Pod
Metadata:
      name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - cat
     tty: true
     volumeMounts:
         - name: docker-config
            mountPath: /kaniko/ .docker
      volumes:
          - name: docker-config
             configMap:
                  Name: docker-config  
    - sleep
    args:
    - 9999999
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: docker-credentials 
          items:
            - key: .dockerconfigjson
              path: config.json
"""
    }
  }
  stages {
    stage('Build with Kaniko') {
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') {
          sh '''#!/busybox/sh
            echo "FROM jenkins/inbound-agent:latest" > Dockerfile
            /kaniko/executor --context `pwd` --destination <docker-username>/hello-kaniko:latest 
          '''
        }
      }
    }
  }
}
