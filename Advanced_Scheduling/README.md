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
            
kubectl label nodes <node-name> disktype=ssd = lable the node
kubectl label node <node-name> <label-key>- = unlable the node

---------------------------------------------------------
# Difference:`nodeName` forces a pod to run on one specific node. `nodeSelector` lets Kubernetes choose any node with matching labels.
--------------------------------------------------------------------------------------------------------------------------------
# Affinity:In Kubernetes, affinity rules help control how pods are scheduled by allowing you to specify rules for where pods should or should not be placed, based on certain conditions. There are two main types:
-------------------------------------------------------------------------------------
 1. NodeAffinity:Controls which nodes a pod can be scheduled on, similar to nodeSelector but more flexible. Node affinity uses requiredDuringSchedulingIgnoredDuringExecution (mandatory rules) and preferredDuringSchedulingIgnoredDuringExecution (soft preferences).

The nodeAffinity section contains the affinity rules for nodes, specifying how Kubernetes should place the pod based on node labels.

# requiredDuringSchedulingIgnoredDuringExecution:This section defines the mandatory conditions for node selection. If these conditions aren't met on any node, the pod will not be scheduled.

nodeSelectorTerms: A list of potential node label conditions. If any term in the list is matched, the pod can be scheduled on that node (i.e., an OR condition across nodeSelectorTerms).
matchExpressions: Within each nodeSelectorTerm, multiple matchExpressions can define multiple key-value conditions. All matchExpressions within a nodeSelectorTerm must match (i.e., an AND condition within matchExpressions).

syntax:
     requiredDuringSchedulingIgnoredDuringExecution:
       nodeSelectorTerms:
         - matchExpressions:
             - key: disktype
               operator: In
               values:
               - ssd
 -------------
# preferredDuringSchedulingIgnoredDuringExecution
This section defines soft conditions for node selection. Kubernetes will try to place the pod on nodes that match these conditions, but if no suitable nodes are available, it will schedule the pod on other nodes.

weight: A number between 1 and 100 that indicates the priority of this preference. Higher weights mean a stronger preference.
preference: Contains matchExpressions similar to requiredDuringSchedulingIgnoredDuringExecution, defining the node label conditions for preference.

synatx: 
       preferredDuringSchedulingIgnoredDuringExecution:
       - weight: 1
         preference:
           matchExpressions:
           - key: region
             operator: In
             values:
             - us-west

 ---------------------
 ## Node affinity provides powerful options to control pod placement in a cluster:

# requiredDuringSchedulingIgnoredDuringExecution: Must match; pods are only scheduled if these conditions are met.
# preferredDuringSchedulingIgnoredDuringExecution: Tries to match for optimal placement but falls back if no nodes meet these preferences.
-----------------------------------------------------------------------
2. PodAffinity:Controls the placement of pods relative to other pods. Pod affinity places pods close to each other (e.g., same node or availability zone), while anti-affinity spreads them apart to avoid single points of failure.
   
4. Pod Anti-Affinity: Use it to distribute pods across nodes for high availability.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Taints & Tolerations:

Taints and tolerations are key concepts in Kubernetes used to control which pods can be scheduled on which nodes. They help ensure that specific workloads are run on appropriate nodes, improving resource management and operational stability.

Taints
A taint is a property of a node that prevents pods from being scheduled onto it unless those pods tolerate the taint. Taints are set on nodes using the following format:

<key>:<value>:<effect>
key: A string that identifies the taint.
value: An optional string that provides additional context.
effect: Specifies what happens to pods that do not tolerate the taint. The possible effects are:
NoSchedule: Pods that do not tolerate the taint cannot be scheduled on the node.
PreferNoSchedule: The scheduler will try to avoid scheduling pods that do not tolerate the taint on the node, but it's not a strict requirement.
NoExecute: Existing pods that do not tolerate the taint are evicted from the node, and new pods cannot be scheduled on it.
--------------------------------------------------
Tolerations
A toleration is a property of a pod that allows it to be scheduled on nodes with matching taints. Tolerations are specified in the pod's configuration in the following format:

tolerations:
- key: "<key>"
  operator: "Equal"  # or "Exists"
  value: "<value>"    # required only if operator is "Equal"
  effect: "<effect>"  # optional
  tolerationSeconds: <seconds> # optional
  
key: The key of the taint that the pod can tolerate.
operator: Indicates how the key and value should be compared. It can be Equal (the value must match) or Exists (the value is not checked).
value: The value of the taint that the pod can tolerate (only required if the operator is Equal).
effect: The effect that this toleration corresponds to.
tolerationSeconds: Optional; defines how long the pod can tolerate the taint.
------------------------------------------------------------------------------------
eg:kubectl taint nodes node1 dedicated=high-availability:NoExecute = taint
   kubectl taint nodes node1 dedicated=high-availability:NoExecute- = untaint
