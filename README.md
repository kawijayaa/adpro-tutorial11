# Reflection on Hello Minikube

1. When the deployment is exposed as a service, it will allow connections from outside. So when we connect to the given URL for the service, the number of logs will increase esince there will be incoming HTTP requests.

2. The purpose of the `-n` flag is to specify the namespace that we want to get. The output will not list the pods/services that we explicitly created since the created pods/services do not belong to the `kube-system` namespace.

# Reflection on Rolling Update & Kubernetes Manifest File

1. Recreate will shutdown the old deployment and recreate the new deployment, this will have some downtime as a side effect. RolllingUpdate will maintain a specific number of instances alive to avoid downtime.

2. I copied the RollingUpdate manifest file and changed the strategy to `Recreate`. Then I ran `kubectl apply -f deployment-recreate.yml` to apply the changes. 

3. This is the manifest for the `deployment-recreate.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "3"
  creationTimestamp: "2024-05-14T06:44:53Z"
  generation: 4
  labels:
    app: spring-petclinic-rest
  name: spring-petclinic-rest
  namespace: default
  uid: ae4b9b6f-8477-40b9-a2e9-70ee95c18071
spec:
  progressDeadlineSeconds: 600
  replicas: 4
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: spring-petclinic-rest
  strategy:
    type: Recreate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: spring-petclinic-rest
    spec:
      containers:
      - image: docker.io/springcommunity/spring-petclinic-rest:3.2.1
        imagePullPolicy: IfNotPresent
        name: spring-petclinic-rest
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 4
  conditions:
  - lastTransitionTime: "2024-05-14T06:49:32Z"
    lastUpdateTime: "2024-05-14T06:49:32Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2024-05-14T06:44:53Z"
    lastUpdateTime: "2024-05-14T06:52:32Z"
    message: ReplicaSet "spring-petclinic-rest-744fbd68bc" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 4
  readyReplicas: 4
  replicas: 4
  updatedReplicas: 4
```

4. The benefits of a manifest file is to easily recreate a deployment without remembering commands. Also, since the manifest file is a YAML file it can also be manipulated programmatically.
