## create certs
```
$ mkdir -p secrets/certs
$ openssl req -nodes -newkey rsa:2048 -keyout secrets/certs/dashboard.key -out secrets/certs/dashboard.csr -subj "/C=/ST=/L=/O=/OU=/CN=kubernetes-dashboard"
$ openssl x509 -req -sha256 -days 365 -in secrets/certs/dashboard.csr -signkey secrets/certs/dashboard.key -out secrets/certs/dashboard.crt
```

## deploy
```
$ kubectl apply -k .
```

## get service-account token
```
$ kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}') -o jsonpath="{.data.token}" | base64 -D
```

## open kubernetes-dashboard
```
$ open https://$(kubectl get nodes --namespace kubernetes-dashboard -o jsonpath="{.items[0].status.addresses[0].address}"):$(kubectl get --namespace kubernetes-dashboard -o jsonpath="{.spec.ports[0].nodePort}" services kubernetes-dashboard)
```
