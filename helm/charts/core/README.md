Build the PHP and Nginx Docker images:
```
docker build -t andysi/personal-dev-environment-php -t andysi/personal-dev-environment-php:latest symfony
docker build -t andysi/personal-dev-environment-nginx -t andysi/personal-dev-environment-nginx:latest -f symfony/Dockerfile.nginx symfony
```
Push your images to your Docker registry, example with Google Container Registry:
```
gcloud docker -- push andysi/personal-dev-environment-php
gcloud docker -- push andysi/personal-dev-environment-nginx
```

Deploy your API to the container:

```
helm install ./symfony/helm/uac --namespace=uac --name uac1 \
    --set php.repository=andysi/personal-dev-environment-php \
    --set nginx.repository=andysi/personal-dev-environment-nginx \
    --set secret=MyAppSecretKey \
    --set mysql.mysqlPassword=MyPgPassword \
    --set mysql.persistence.enabled=true \
    --set corsAllowUrl='^https?://[a-z\]*\.uac.com$'
```
