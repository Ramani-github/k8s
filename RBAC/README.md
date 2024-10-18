# k8s-RBAC

openssl genrsa -out ramani.key 2048

openssl req -new -key ramani.key -out ramani.csr -subj "/CN=ramani/O=development"

openssl x509 -req -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -in ramani.csr -out ramani.crt -days 365

kubectl config set-credentials ramani --client-certificate=ramani.crt --client-key=ramani.key

kubectl config set-context ramani-k8s --cluster=kubernetes --user=ramani --namespace=default

kubectl config get-contexts

kubectl config use-context ramani-k8s

kubectl api-resources -o wide | grep pod

kubectl get role

rolebinding = subject(user) + role (activities to do on pods)

ClusterRole and ClusterRoleBinding so there must be a way to define roles and role bindings at the cluster level instead of namespace level that's  where cluster role and cluster role binding comes into the picture the only difference between role bending and cluster role bending is that role  binding is namespaced meaning only where role binding is defined that permissions are valid whereas cluster role bending is at the cluster level that means they can access the resources in any
namespace 

In Kubernetes, a Service Account is an identity used by applications or services running inside a pod to interact with the Kubernetes API. It's a way to grant pods specific permissions (RBAC - Role-Based Access Control) to perform actions on Kubernetes resources such as secrets, config maps, and other resources.
By default, every pod is assigned the default service account in the namespace where it is running. However, for more secure or specific access, you can create custom service accounts with specific roles and permissions.

  ku create ns test

  ku run nginx --image=nginx -n test

  ku create sa test-sa
 
  ku exec -it kubectl-pod -- bash
  ku auth can-i create pods
  ku auth can-i create ns
  ku auth can-i create svc
  ku auth can-i delete pods
  kubectl exec -it kubectl-pod -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
