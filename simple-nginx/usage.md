
# Deployment version

```
// apply
kubectl apply -f nginx-deployment.yaml

//local port forward on 3000
kubectl port-forward svc/nginx-service 3000:80 -n xgo-test

// => http://localhost:8080

// use curl to spam a few requests
for i in {1..10}; do curl -s http://localhost:3000 --header "Connection: close" | grep hostname; done

// OR use an internal pod to make curl calls ()
// run busy box
kubectl run -n xgo-test curlpod --rm -it --image=busybox -- /bin/sh
// in the prompt
wget -qO- http://nginx-service


// remove namespace (delete all)
kubectl delete namespace xgo-test

```

# StatefulSet version

```

kubectl apply -f nginx-statefulset.yaml

// Check status
kubectl get statefulset -n xgo-test nginx

// Scale
kubectl scale statefulset -n xgo-test nginx --replicas=4

```