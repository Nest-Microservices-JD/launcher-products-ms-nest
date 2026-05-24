## Developments

1. Clone the repository
2. Create a .env file
3. Run `docker compose up --build`

## Docker Compose Build Prod

1. `docker build -f Dockerfile.prod -t <image_name> .`
2. `docker compose -f docker-compose.prod.yml build`

## Kubernetes Deployments

# Helm Commands

- Create configuration: `helm create <name>`
- Apply initial configuration: `helm install <name> .`
- Apply updates: `helm upgrade <name> .`

# K8s Commands

- Get pods, deployments, and services: `kubectl get <pods | deployments | services>`
- Review all pods: `kubectl describe pods`
- Review a pod: `kubectl describe pod <name>`
- Delete pod: `kubectl delete pod <name>`
- Check logs: `kubectl logs <name>`
- Restart pod: `kubectl rollout restart deployment <name>`

# Create Deployment:

```
kubectl create deployment <name> --image=<registry/url/image> --dry-run=client -o yaml > deployment.yml
```

# Create service

```
kubectl create service clusterip <name> --tcp=<8888> --dry-run=client -o yaml > service.yml
**kubectl create service nodeport <name> --tcp=<3000> --dry-run=client -o yaml > service.yml**
```

- **clusterip**: can only be accessed from within the cluster
- **nodeport**: can be accessed from outside the cluster

## Secrets

- Create secrets, multiple at once, or one by one.

```
kubectl create secret generic <name> --from-literal=key=value

kubectl create secret generic secret1 --from-literal=key1=value1 --from-literal=key2=value2
```

- Get the secrets `kubectl get secrets`
- View the content of a secret `kubectl get secrets <name> -o yaml`

## Edit a secret

The easiest way is to delete it and recreate it, but if there is more than one secret, we don't want to lose the others.
Remember that secrets are in `base64`, so if we want to edit a secret, we must do it in `base64`.

1. Edit the secret with `kubectl edit secret <name>` this will invoke the editor
2. Change the value (you can use an online editor [to convert to base64](https://www.rapidtables.com/web/tools/base64-decode.html))
3. Press **i** to insert lines and edit the file
4. Put the value to decode on a new line
5. Press **esc** and then `:. ! base64 -D` to decode the value
6. Press **i** to insert or edit the value
7. Press **esc** and then `:. ! base64` to encode the value
8. Edit the file again **i** and leave the line in its position
9. Press **esc** and then **:wq** to save and exit

## Configure Google Cloud secrets to obtain images

1. Create secret:

```
kubectl create secret docker-registry gcr-json-key --docker-server=GOOGLE-SERVER-docker.pkg.dev --docker-username=_json_key --docker-password="$(cat 'PATH/DE/<name> IAM.json')" --docker-email=your_email@gmail.com
```

2. Secret path to use the key:

```
kubectl patch serviceaccounts default -p '{ "imagePullSecrets": [{ "name":"gcr-json-key" }] }'
```

## Export and apply configurations with files (secrets in this case)

- To export the configuration files

```
kubectl get secret <name> -o yaml > <name>.yml
```

- Apply the configuration based on the file

```
kubectl create -f <name>.yml
```

## Possible errors

- Error: INSTALLATION FAILED: unable to build Kubernetes objects from release manifest: error parsing : error converting YAML to JSON: yaml: invalid leading UTF-8 octet

--> Execute: `kubectl create deployment client-gateway --image=us-east1-docker.pkg.dev/ms-store-459001/ms-store-registry/client-gateway-production --dry-run=client -o yaml | Out-File ./deployment.yml -Encoding UTF8`