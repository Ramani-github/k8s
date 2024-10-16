# k8s-RBAC

openssl genrsa -out ramani.key 2048

openssl req -new -key ramani.key -out ramani.csr -subj "/CN=ramani/O=development"

openssl x509 -req -CA /etc/kubernetes/pki/ca.crt  ca.crt -CAkey/etc/kubernetes/pki/ca.key ca.key  -CAcreateserial -in ramani.csr -out ramani.crt -days 365

kubectl config set-credentials ramani --client-certificate=ramani.crt --client-key=ramani.key

kubectl config set-context ramani-k8s --cluster=kubernetes --user=ramani --namespace=default

kubectl config get-contexts

kubectl config use-context ramani-k8s

rolebinding = subject(user) + role (activities to do on pods)
