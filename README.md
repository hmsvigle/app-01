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
## 4. Create helm package & Index it
  * Create Package
  ```bash
  $ cd app-01/
  $ helm package tibco/chart/
  Successfully packaged chart and saved it to: /root/app-01/app-01-0.1.0.tgz
  ```
  * Push package to helm_repo git
  ```bash
  cp /root/app-01/app-01-0.1.0.tgz /root/helm_repo/
  git add .
  git commit -m "helm package for app-01 0.1.0 version"
  git push origin 
  ```
  * Create/Merge index in helm_repo    
  ```bash
  # Creating Index if it is first time. Any name can be given for the repo
  $ helm repo index hmsvigle --url https://himansu.github.io/helm_repo/ 
  # Merge Index
  $ helm repo index . --merge index.yaml --url https://hmsvigle.github.io/helm_repo/
  ```
  * Index will contain package details like : `Package url with .tgz file reference.` , `appVersion` , `Package Name`
  ```yaml
  apiVersion: v1
  entries:
    app-01:
    - apiVersion: v2
      appVersion: 1.0.0
      created: "2024-07-24T20:24:55.352264332Z"
      description: App-01
      digest: 9143100f570e11dd994fe4eecb65f50762d239c60165396852b5026f7d1bfb0a
      name: app-01
      type: application
      urls:
      - https://hmsvigle.github.io/helm_repo/app-01-0.1.0.tgz
      version: 0.1.0
  ```
## 5. Test helm package 
* Add helm repo & update repo. While adding repo, url is imp. name can be anything based on our convenience.
```yaml
$ helm repo add hmsvigle https://hmsvigle.github.io/helm_repo/
"hmsvigle" successfully added
$ helm repo update hmsvigle
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "hmsvigle" chart repository
Update Complete. ⎈Happy Helming!⎈
```
* Search for required package from the repo
```yaml
$ helm search repo 
NAME                            CHART VERSION   APP VERSION     DESCRIPTION                
hmsvigle/app-01                 0.1.0           1.0.0           App-01                     
hmsvigle/app-02                 0.1.0           1.0.2           A Helm chart for Kubernetes
```
* Install helm package
```yaml
$ helm install app-01 hmsvigle/app-01 -n stg \
-f tibco/apps/app-01/values.yaml \
-f tibco/environments/stg-values.yaml \
-f tibco/apps/app-01/environments/stg-values.yaml \
--set appNameGeneric=app-01 \
--set appName=app-01
```
