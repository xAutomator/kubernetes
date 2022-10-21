# Kubernetes Scheduling Cheatsheet

Learning Checklist
- [x] Taints and Tolerations
  -  [ ] Definitions and notes
  -  [ ] Examples
- [ ] Node Afinity
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

### Resource Limits


### Daemon sets vs deployments vs jobs