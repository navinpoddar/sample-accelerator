# start-spring-accelerator
# experimental app accelerator to emulate spring initializer

## pre-requisites
This app accelerator depends on the following fragments deployed before it will work

### prepare environment
```
export IMAGE_REGISTRY_SERVER=[YOUR_IMAGE_REGISTRY_SERVER]
export IMAGE_REGISTRY_USERNAME=[YOUR_IMAGE_REGISTRY_USERNAME]
export IMAGE_REGISTRY_PASSWORD=[YOUR_IMAGE_REGISTRY_PASSWORD]
export GIT_USERNAME=[GIT REPO USERNAME]
export GIT_PERSONAL_ACCESS_TOKEN=[GIT PERSONAL ACCESS TOKEN]
```

### local path accelerator development
```
Docker login to target registry to push accelerator code to the image repository 
example:
    docker login -u %IMAGE_REGISTRY_USERNAME --password-stdin $IMAGE_REGISTRY_SERVER < %IMAGE_REGISTRY_PASSWORD
Create registry credentials for accelerator resource to pull image source

example: 
	tanzu secret registry add registry-credentials --username %IMAGE_REGISTRY_USERNAME --password %IMAGE_REGISTRY_PASSWORD --server %IMAGE_REGISTRY_SERVER --namespace accelerator-system

Change the directory to the accelerator folder you want to iterate on

Create Accelerator using Tanzu CLI 

example:
    tanzu accelerator create test-accelerator -n accelerator-system --local-path . --display-name=test --source-image $IMAGE_REGISTRY_SERVER/test-accelerator --secret-ref registry-credentials --interval 10s

Iterate over accelerator

Push local changes to the accelerator to the image repository

example: 
    tanzu accelerator push --local-path . --source-image $IMAGE_REGISTRY_SERVER/test-accelerator
```
## deployment the accelerator to github when finished editing
to deploy this accelerator via github repository, commit the local-path to github and ensure the sample-accelerator-deployment.yaml refers to that directory

`kubectl delete acc test-accelerator -n accelerator-system`

`kubectl create secret generic git-secret --namespace accelerator-system --from-literal=username=$GIT_USERNAME --from-literal=password=$GIT_PERSONAL_ACCESS_TOKEN`
    
`kubectl apply -f sample-accelerator-deployment.yaml`

consider reducing the interval for refreshing the repository
