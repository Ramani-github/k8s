kubectl create ns a
kubectl create ns b

kubectl label ns a nsp=a
kubectl label ns b nsp=b

kubectl run nginx1 --image nginx:latest -n default
kubectl run nginx2 --image nginx:latest -n default

kubectl run -n a a1 --image nginx -l ns=a
kubectl run -n a a2 --image nginx -l ns=a

kubectl run -n b b1 --image nginx -l ns=b
kubectl run -n b b2 --image nginx -l ns=b

kubectl get po -n a -o wide  && kubectl get po -n b -o wide 

--------------------------
controlplane $ kubectl get po -n a -o wide  && kubectl get po -n b -o wide  && kubectl get po -owide
NAME   READY   STATUS    RESTARTS   AGE   IP            NODE           NOMINATED NODE   READINESS GATES
a1     1/1     Running   0          25m   192.168.0.9   controlplane   <none>           <none>
a2     1/1     Running   0          25m   192.168.0.8   controlplane   <none>           <none>
NAME   READY   STATUS    RESTARTS   AGE   IP             NODE           NOMINATED NODE   READINESS GATES
b1     1/1     Running   0          25m   192.168.0.10   controlplane   <none>           <none>
b2     1/1     Running   0          25m   192.168.0.11   controlplane   <none>           <none>
NAME     READY   STATUS    RESTARTS   AGE   IP            NODE           NOMINATED NODE   READINESS GATES
nginx1   1/1     Running   0          25m   192.168.0.6   controlplane   <none>           <none>
nginx2   1/1     Running   0          25m   192.168.0.7   controlplane   <none>           <none>
------------------------------
# Checking communication from a to Default

kubectl exec -it  a1 -n a -- ping  192.168.0.7(default ns)

# checking communication from  default to a ,b 
kubectl exec -it  nginx1  -- ping 192.168.0.9 \
&& kubectl exec -it  nginx1  -- ping 192.168.0.8 \
&& kubectl exec -it  nginx1  -- ping 192.168.0.10 \
&& kubectl exec -it  nginx1  -- ping 192.168.0.11 

# checking communication from  a  to b 
kubectl exec -it  a1 -n a -- ping 192.168.0.10 &&  kubectl exec -it  a2 -n a -- ping 192.168.0.11

# checking communication from b to a
kubectl exec -it  b1 -n b -- ping 192.168.0.9 &&  kubectl exec -it  b2 -n b -- ping 192.168.0.8

