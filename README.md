# hawk

**h**elm **aw**s **k**ubectl

docker image with helm, aws-cli and kubeclt

How did I come up with this name ?
http://www.allscrabblewords.com/unscramble/helmkubeaws  
Probably not good for SEO :(


## Motivation

To achieve CI/CD through Jenkins (non k8s) for deploying apps to AWS EKS using Helm.

## Usage

```bash
docker run -it \
  -e AWS_ACCESS_KEY_ID="<AWS key>" \
  -e AWS_SECRET_ACCESS_KEY="<AWS secret>" \
  -e AWS_DEFAULT_REGION="us-east-1" \
  naveenrajm/hawk:latest

# configure kubectl auth for an existing EKS cluster named "my-cluster"
aws eks update-kubeconfig --name my-cluster

# confirm it worked by listing your pods
kubectl get pods --all-namespaces

# or list your helm deployments
helm ls --all-namespaces
```

### To use in CI 

```bash
# add how use this image in Jenkins
```
## How to build:

To build your custom image, with desired versions , adding or deleting packages.
Three reasons to build your own image
* versions - select your versions for helm , aws and kubectl ( [See available versions](#Available-Versions:)
* add packages 
* remove unwanted packages  
> Caution: some packages depend on each other , like aws needs glibc, groff, and less. So be cautious while removing

>for better readability each packages are in different lines in Dockerfile 

```bash
git clone hawk
cd hawk
# change versions , add or remove pakages  
docker build -t myhawk:v1.0.0 .
```

## Available Versions:

* **h**elm    https://github.com/helm/helm/releases
* **aw**s-cli https://github.com/aws/aws-cli/releases
* **k**ubectl https://github.com/kubernetes/kubernetes/releases



**Helm warining Fix:** 

If you see this waring for helm
```bash
bash-5.0# helm version
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /root/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /root/.kube/config
```
https://github.com/helm/helm/issues/8776
```bash
chmod go-r ~/.kube/config
```

## References:

* https://github.com/aws/aws-cli/issues/4685#issuecomment-615872019
* https://github.com/dtzar/helm-kubectl
* https://github.com/jshimko/kube-tools-aws
