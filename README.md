# Helm Example Repository
## 1. helm_repo structure
  * Replicated helm_ngil repo
  * application structure is as bellow
  ````yaml
    tibco
        |-- apps
        |   `-- app-01
        |       |-- configmap.yaml
        |       `-- values.yaml
        |-- chart
        |   |-- Chart.yaml
        |   |-- app-01
        |   |   |-- configmap.yaml
        |   |   |-- environments
        |   |   |   |-- reg-values.yaml
        |   |   |   `-- stg-values.yaml
        |   |   `-- values.yaml
        |   `-- templates
        |       |-- _helpers.tpl
        |       |-- configmap.yaml
        |       |-- deployment.yaml
        |       `-- service.yaml
        `-- environments
            |-- reg-values.yaml
            `-- stg-values.yaml

  ````
## 2. Application files with global, static & overriding values
  * 

## 3. helm upgrade command tested
  * helm command 
  ````yaml
  helm upgrade --install  app-01 tibco/chart/ -n stg \
  -f tibco/chart/app-01/values.yaml \
  -f tibco/environments/stg-values.yaml \
  -f tibco/chart/app-01/environments/stg-values.yaml \
  --set appNameGeneric=app-01 \
  --set appName=app-01

  ````
  * Output
  ````bash
    Release "app-01" has been upgraded. Happy Helming!
    NAME: app-01
    LAST DEPLOYED: Fri Jul  5 07:49:57 2024
    NAMESPACE: stg
    STATUS: deployed
    REVISION: 2
    TEST SUITE: None
  ````
## 4. create helm repo & push to registry

## 5. Test helm package 

Add this repository to Helm.

```
helm repo add examples https://helm.github.io/examples
```

Install an example.

```
helm install ahoy examples/
```
