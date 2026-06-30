## Should look something like ...
```yaml
spec:
      containers:
      - args:
        - --config
        - https://telegraf-controller01.lab.tld/api/configs/60238f96-f2b9-4efd-b83b-fc8eaa435190/toml
        - --config-url-watch-interval
        - 10m
```

Or update via kubectl
# 1.
```
kubectl -n influxdb3 create secret generic telegraf-controller-token \
--from-literal=token="tc-apiv1_12345678" \
--dry-run=client -o yaml | kubectl apply -f -
```
# 2.
```
kubectl -n influxdb3 patch ds/telegraf-ds \
--type='json' \
 -p='[
   {
     "op": "add",
     "path": "/spec/template/spec/containers/0/env/-",
     "value": {
      "name": "TELEGRAF_CONTROLLER_TOKEN",
       "valueFrom": {
         "secretKeyRef": {
           "name": "telegraf-controller-token",
           "key": "token"
         }
       }
     }
   }
 ]'
```
