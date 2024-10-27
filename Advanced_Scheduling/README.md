# Nodename:To specify a node for a pod in Kubernetes, you can use a simple manifest file with a nodeName field. This field tells Kubernetes to schedule the pod on a specific node in the cluster
 syntax
      spec:
        nodeName: my-node
-------------------------------------------
# Nodeselector:The nodeSelector field in a Kubernetes pod manifest allows you to schedule pods onto specific nodes based on labels, rather than targeting a single node by name. This approach is useful for workloads that need to run on nodes with specific characteristics (e.g., availability zone, hardware requirements).
syntax
        spec:
          nodeSelector:
            disktype: ssd
            
kubectl label nodes <node-name> disktype=ssd
---------------------------------------------------------
# Difference:`nodeName` forces a pod to run on one specific node. `nodeSelector` lets Kubernetes choose any node with matching labels.
--------------------------------------------------------------------------------------------------------------------------------
# Affinity:In Kubernetes, affinity rules help control how pods are scheduled by allowing you to specify rules for where pods should or should not be placed, based on certain conditions. There are two main types:
 1. NodeAffinity:Controls which nodes a pod can be scheduled on, similar to nodeSelector but more flexible. Node affinity uses requiredDuringSchedulingIgnoredDuringExecution (mandatory rules) and preferredDuringSchedulingIgnoredDuringExecution (soft preferences).
2. PodAffinity:Controls the placement of pods relative to other pods. Pod affinity places pods close to each other (e.g., same node or availability zone), while anti-affinity spreads them apart to avoid single points of failure.
3. Pod Anti-Affinity: Use it to distribute pods across nodes for high availability.


