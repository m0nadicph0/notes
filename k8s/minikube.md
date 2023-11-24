# Minikube cheatsheet

## Start minikube cluster

```
$ minikube start
```
## Open Dashboard

```
$ minikube dashboard
```

## Create a deployment

```
$ kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
$ kubectl get deployments
```
## View Pods

```
$ kubectl get pods
```

## View cluster events

```
$ kubectl get events
```

## View the kubectl configuration

```
$ kubectl config view
```

## View logs

```
$ kubectl logs hello-node-5f76cf6ccf-br9b5
```

## Create a service

```
$ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
$ kubectl get services
```
## View app in browser

```
$ minikube service hello-node
```

## Cleanup

```
$ kubectl delete service hello-node
$ kubectl delete deployment hello-node
$ minikube stop
$ minikube delete
```








