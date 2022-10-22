# Kubernetes Scheduling Cheatsheet

Learning Checklist
- [x] [Taints and Tolerations](#tains-and-tolerations)
  -  [ ] Definitions and notes
  -  [ ] Examples
- [ ] [Node Afinity](#node-and-pod-affinity)
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


### Node And Pod Affinity

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

The affinity feature consists of two types of affinity:
- Node affinity functions like the nodeSelector field but is more expressive and allows you to specify soft rules.
- Inter-pod affinity/anti-affinity allows you to constrain Pods against labels on other Pods.

Node affinity weight
You can specify a weight between 1 and 100 for each instance of the preferredDuringSchedulingIgnoredDuringExecution affinity type. When the scheduler finds nodes that meet all the other scheduling requirements of the Pod, the scheduler iterates through every preferred rule that the node satisfies and adds the value of the weight for that expression to a sum.

The final sum is added to the score of other priority functions for the node. Nodes with the highest total score are prioritized when the scheduler makes a scheduling decision for the Pod.

For example, consider the following Pod spec:



<b>Node Affinity


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-affinity-anti-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/os
            operator: In
            values:
            - linux
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: label-1
            operator: In
            values:
            - key-1
      - weight: 50
        preference:
          matchExpressions:
          - key: label-2
            operator: In
            values:
            - key-2
  containers:
  - name: with-node-affinity
    image: registry.k8s.io/pause:2.0
```


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

Deploy a deployment to any node that has the "node-role.kubernetes.io/control-plane" label

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      color: red
  template:
    metadata:
      labels:
        color: red
    spec:
      containers:
      - name: nginx
        image: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/control-plane
                operator: Exists
```
### Resource Limits


### Daemon sets vs deployments vs jobs

Deployment have different update strategis