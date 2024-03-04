# NOTE FROM ME

I was struggling build the image that work correctly from `mac M1` and push to `AKS`.

I update 1st line of both the `DockerfileProducts` and `DockerfileStore`.
From 
```docker
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
```
To 
```docker
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
```
and then execute these commands to build and push images to Azure Container Registry.

```shell
docker buildx build --push --builder=tasani-builder \
  --platform="linux/amd64,linux/arm64" \
  -t $ACR_NAME.azurecr.io/productservice:v1 \
  -f DockerfileProducts .

docker buildx build --push --builder=tasani-builder \
  --platform="linux/amd64,linux/arm64" \
  -t $ACR_NAME.azurecr.io/storeimage:v1 \
  -f DockerfileStore .
```

Useful links:
- [Docker build multiplatform](https://www.youtube.com/watch?v=hVRsqk_iKHA)
- [Improving multi-platform container support](https://devblogs.microsoft.com/dotnet/improving-multiplatform-container-support/)
- [Manually deploy your cloud-native app to Azure Kubernetes Service](https://learn.microsoft.com/en-us/training/modules/microservices-devops-aspnet-core/2-exercise-manually-deploy-cloud-native-application)
