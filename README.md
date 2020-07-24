# jenkins-helm

### Export pod name from existing jenkins instance
```
export POD_NAME=$(kubectl get pods --namespace jenkins -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=my-release" -o jsonpath="{.items[0].metadata.name}")
```

### Copy jenkins secret files
```
kubectl -n jenkins cp $POD_NAME:/var/jenkins_home/secrets/master.key master.key
kubectl -n jenkins cp $POD_NAME:/var/jenkins_home/secrets/hudson.util.Secret hudson.util.Secret
kubectl -n jenkins cp $POD_NAME:/var/jenkins_home/credentials.xml credentials.xml
```

### Create kubernetes secrets for jenkins
```
kubectl create secret generic jenkins-credentials --from-file=credentials.xml -n jenkins
kubectl create secret generic jenkins-secrets --from-file=hudson.util.Secret --from-file=master.key -n jenkins
kubectl create secret generic docker-registry-kaniko --from-file=config.json -n jenkins
```

### Install jenkins 
```
helm install my-release stable/jenkins -n jenkins -f values-override.yaml
```