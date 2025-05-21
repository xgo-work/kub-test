# Cheatsheet

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

```
kubectl get <pods | pvc | svc | ns | ... > -A -o wide

// all objects from a namespace
kubectl get all -n wegas

kubectl describe pod -n <namespace> <pod name>

kubectl -n <namespace> exec -ti <pod name> -- bash

kubectl logs -f -n wegas wegas-payara-6b65c6bcb6-nqtj6

kubectl edit <deploy | pod | ... > -n namespace name
kubectl edit deploy -n wegas-test wegas-payara

kubectl port-forward nginx-deployment-xgo-85996f8dbd-m98v4 8080:80

kubectl delete pod wegas-payara-6b69455b9-m7997

kubectl config current-context

kubectl create namespace wegas-test

// disable (cordon) scheduling on node
kubectl cordon fmfcqbcgqv-worker-fn5q5v-75f9765fc4-5pqsm
// enable
kubectl uncordon fmfcqbcgqv-worker-fn5q5v-75f9765fc4-5pqsm

// restart a deployment
kubectl rollout restart -n wegas deployment/wegas-payara
kubectl rollout restart -n colab deployment/colab-payara

// delete one pod (which will restart)
kubectl delete

// TRAFFIC ANALYZER
kubectl port-forward -n kube-system svc/hubble-ui 8080:80

// DRAIN
kubectl drain --ignore-daemonsets --delete-emptydir-data vlmxl8cqk7-worker-cvpnp-78b58dd9cd-t9bps

// Pods sorted by restart count
kubectl get pods -A --sort-by='{.status.containerStatuses[*].restartCount}'

// EVENTS
kubectl get events -A

// CRON JOBS
kubectl get cronjobs -A

// ALL JOBS
kubectl get jobs -A
// delete JOB
kubectl delete job -n <namespace> <jobname> (job name not the pod)

// MANUAL BACKUP
kubectl create job -n wegas --from=cronjob/wegas-backup-cronjob wegas-backup-cronjob-manual-N

// CAT INGRESS CONTROLLER CONFIG
kubectl exec ingress-nginx-controller-558f7d45d6-2p2gz -n ingress-nginx -- cat /etc/nginx/nginx.conf

// Edit Ingress live
kubectl edit ingress -n wegas wegas

// Nodes
kubectl get nodes
kubectl describe node test-cluster-worker11-9b67c67d7-ff8nn
kubectl get pods --all-namespaces -o wide --field-selector spec.nodeName=fmfcqbcgqv-worker-j6w5l-85fc47bd59-2nbhq

// network policies
kubectl get networkpolicy -A
kubectl edit networkpolicy -n wegas-jdk17 allow-ingress-access-monitoring-jdk17


// DEBUG with hubble
kubectl port-forward -n kube-system svc/hubble-ui 8080:80

// connect to DB
kubectl exec -ti -n wegas wegas-psql-ff9bd76cb-zvlft -- bash
psql -U user -d wegas_dev

//Network policies
kubectl get networkpolicy -n <>
kubectl describe networkpolicy -n wegas allow-egress-traffic
```
## GRAFANA

`kubectl port-forward -n monitoring svc/prom-stack-grafana 8090:80`

## HELM
```
helm upgrade --install colab charts/coLAB/v1.1.0/ --namespace colab

helm list -A
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm repo list

redeploy

helm upgrade --reuse-values prom-stack prometheus-community/kube-prometheus-stack --version XXXX -n monitoring --values config.yaml
```

# Backup procedure
https://github.com/Heigvd/WegasDocker/blob/master/k8s_cluster_setup_doc/backup-restore.md


```
