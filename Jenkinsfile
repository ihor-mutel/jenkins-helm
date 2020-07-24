// wwww

String podDefinition = """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
    - name: maven
      image: maven:alpine
      command:
        - cat
      tty: true
    - name: busybox
      image: busybox
      command:
        - cat
      tty: true
      volumeMounts:
        - mountPath: /kaniko/.docker
          name: volume-0
  volumes:
    - name: volume-0
      secret:
        defaultMode: 420
        secretName: docker-registry-kaniko
"""

pipeline {

    triggers {
        pollSCM 'H/1 * * * *'
    }
    agent {
        kubernetes {
            yaml podDefinition
        }
    }
    stages {
        stage('Run maven') {
            steps {
                container('maven') {
                    sh 'mvn -version'
                }
                container('busybox') {
                    sh '/bin/busybox'
                }
            }
        }
    }
}