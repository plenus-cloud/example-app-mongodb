# Example app + MongoDB helm chart

This helm chart has a deployment for a generic application, linked to a mongodb deployment, using bitnami/mongodb helm chart.

# Requirements

helm 3

# helm repository

First you need to add Plenus helm repository

```
helm repo add plenus https://plenus-charts.storage.googleapis.com/stable/
helm repo update
```

# Usage

At the bare minimum you need to configure the values image.repository and image.tag to specify which image to use in the deployment.

```
helm install my-release plenus/example-app-mongodb --namespace=my-namespace --set image.repository=nginxdemos/hello --set image.tag=0.2 --wait --atomic
```

If you need to configure environment variables for the deployment you have to use a values files, for example:

```
cat <<EOF > values-express.yaml
image:
  repository: mongo-express
  tag: 0.54.0

deployment:
  # port exposed in the container
  # this will be used also in the service to route requests to the deployment
  port: 8081

envs:
  ME_CONFIG_MONGODB_SERVER: "express-mongodb"
  ME_CONFIG_MONGODB_ADMINUSERNAME: "root"
  ME_CONFIG_MONGODB_ADMINPASSWORD: "supersecret"

mongodb:
  auth:
    rootPassword: "supersecret"
EOF
```

```
helm install express plenus/example-app-mongodb --namespace=my-namespace --values values-express.yaml --wait --atomic
```

This will deploy a release called "express" with a mongo-express application that connects to the mongodb, it also set some variables
for the deployment and the mongodb root password.
The variable ME_CONFIG_MONGODB_SERVER in this example points to the service name for mongodb.
