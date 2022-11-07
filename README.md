# k8s-intro

## Prereqs 

```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

## Init

Create local cluster

```
k3d cluster create
```

Assign hostnames to local cluster (modify local DNS)

```
hostnames="k8s.local nginx.local"
ip=$(docker network inspect k3d-k3s-default | jq -r '.[0].Containers[] | select(.Name=="k3d-k3s-default-server-0") | .IPv4Address' | cut -d "/" -f1)
for host in $hostnames; do sudo sed -i "/${host}/d" /etc/hosts; echo "${ip} ${host}" | sudo tee -a /etc/hosts; done
```

## Create and play with resources

Apply yaml files for resources

```
curl -sSf https://raw.githubusercontent.com/tenstad/k8s-intro/main/deployment.yaml | kubectl apply -f -
curl -sSf https://raw.githubusercontent.com/tenstad/k8s-intro/main/service.yaml | kubectl apply -f -
curl -sSf https://raw.githubusercontent.com/tenstad/k8s-intro/main/ingress.yaml | kubectl apply -f -
```

Modify a running container

```
kubectl exec -it $(kubectl get pods | grep nginx | head -n 1) -- bash
apt update && apt install -y nano
nano /usr/share/nginx/html/index.html
nginx -s reload
```

Delete pods

```
kubectl get pods | grep nginx | tail -n 2 | awk '{print $1}' | xargs kubectl delete pod
```

Kill main process in running container

```
apt update && apt install -y procps
pkill nginx
```

## Cleanup

Delete local cluster

```
k3d cluster delete
```
