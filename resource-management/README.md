-request:Nodes
-limit:POds
# request we Define the minimum resources that should be available on a node to schedule a part and
# limits we Define the maximum resources a pod can use 
# if we don't Define requests and limits the kubernetes scheduler May schedule all our parts onto the same node without triggering any auto scaling which will eventually make the kubernetes node overloaded and unstable and our application performance will suffer 

eksctl enable addons metrics-server or 

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.1/components.yaml

kubectl top pods

kubectl top nodes
