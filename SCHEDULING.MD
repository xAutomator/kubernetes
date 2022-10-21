# Kubernetes Scheduling Cheatsheet

Learning Checklist
- [x] [Taints and Tolerations](#tains-and-tolerations)
  -  [ ] Definitions and notes
  -  [ ] Examples
- [ ] [Node Afinity](#node-afinity)
  -  [ ] Definitions and notes
  -  [ ] Examples
- [ ] Resource Limits
  -  [ ] Definitions and notes
  -  [ ] Examples
- [ ] [Daemon sets vs deployments vs jobs](Daemon sets vs deployments vs jobs)
  -  [ ] Definitions and notes
  -  [ ] Examples





You can use any of the following methods to choose where Kubernetes schedules specific Pods:
- [nodeSelector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector) field matching against [node label](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#built-in-node-labels)
- [Affinity and anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)
- [nodeName field](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename)
- [Pod topology spread constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#pod-topology-spread-constraints)


To Label a node use the following command
                   
    kubectl label nodes <your-node-name> labelName=labelValue


### Taints and Tolerations


### Node Afinity

Node <b>Afinity</b> Operators
- In
- NotIn
- Exists
- DoesNotExist 
- Gt
- Lt

Node <b>ANTI-Afinity</b> Operators
- NotIn
- DoesNotExist

There are four node afinity execution rules:

- requiredDuringSchedulingIgnoredDuringExecution
- preferredDuringSchedulingIgnoredDuringExecution
- requiredDuringSchedulingrequiredDuringExecution - Future state
- preferredDuringSchedulingrequiredDuringExecution - Future state



###### Examples

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-pod-affinity
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - S1
        topologyKey: topology.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: security
              operator: In
              values:
              - S2
          topologyKey: topology.kubernetes.io/zone
  containers:
  - name: with-pod-affinity
    image: registry.k8s.io/pause:2.0
```

KodeKloud Pod Affinity Task 6

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue

```
### Resource Limits


### Daemon sets vs deployments vs jobs