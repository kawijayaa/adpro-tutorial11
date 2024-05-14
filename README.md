# Reflection on Hello Minikube

1. When the deployment is exposed as a service, it will allow connections from outside. So when we connect to the given URL for the service, the number of logs will increase esince there will be incoming HTTP requests.

2. The purpose of the `-n` flag is to specify the namespace that we want to get. The output will not list the pods/services that we explicitly created since the created pods/services do not belong to the `kube-system` namespace.
