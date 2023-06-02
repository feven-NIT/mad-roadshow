Repo for a todo app statefull application with Quarkus and Postgresql

# Quickstart

```shell
git clone https://github.com/feven-redhat/statefull-basic-app.git
cd statefull-basic-app
```

## local environment

```shell
podman run --name postgres-db -e POSTGRES_USER=feven -e POSTGRES_PASSWORD=redhat -p 5432:5432 -d postgres
```

```shell
mvn quarkus:dev
```

## Prerequisites

Create a namespace and label the namespace if you want to use it to deploy the application with argocd.

```shell
oc new-project mad-roadshow
oc label namespace mad-roadshow argocd.argoproj.io/managed-by=openshift-gitops
```

## Openshift


```shell
oc apply -f manifest/applications/application.yaml
oc apply -f manifest/apps/todo-app.yaml
oc apply -f  manifest/apps/postgres.yaml
oc apply -f manifest/quarkus-app-config.yaml
oc create secret generic postgres-credentials -n mad-roadshow    --from-literal=POSTGRES_USER=task-user     --from-literal=POSTGRES_PASSWORD=mysecretpassword     --from-literal=POSTGRES_DB=task     --from-literal=POSTGRES_PORT=5432
```
Get the url

```shell
oc get route -n mad-roadshow -o json | jq -r '.items[0].spec.host' | sed 's/^/https:\/\//'
```

## Argocd

```shell
oc apply -f manifest/applications/project.yaml
oc apply -f manifest/applications/application.yaml
```


# Clean

```shell
oc delete ns mad-roadshow
```
 
