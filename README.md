# Enforce Automated Kubernetes Cluster using Kyverno and ArgoCD

The Kyverno is used to enforce policies, governence and compliance on your kubernetes cluster. The kubernetes cluster would work regardless of the platform its on like AWS, Azure, GCP or on-premises, this project will work without any additional changes.

With the Kyverno Configuration we can do

Generate -> For example, Create a default network policy whenever a namespace is created.

Validate -> For example, Block users from using latest tag in the deployment or pod resources.

Mutate -> For example, Attach pod security policy for a pod that is created without any pod security policy configuration.

Verify Images -> For example, Verify if the Images used in the pod resources are properly signed and verified images.

## High Level Design 

A DevOps Engineer will write the required Kyverno Policy custom resource and commits it to a Git repository. Argo CD which is pre configured with auto-sync to watch for resources in the git repo, deploys the Kyverno Policies on to the Kubernetes cluster.

![kyverno_stack](https://github.com/adityadhopade/kyverno-k8-argocd/assets/48392204/7ef72956-8acf-43a3-bcbf-0cf7bccdcb8d)


## Follow the steps below to get better results

## Installation of Kyverno Controller with ArgoCD
To setup this project you need to install Argo CD controller and Kyverno controller, Assuming you have Kubernetes installed.

Installation of both Kyverno and Argo CD are pretty straight forward as both of them support Helm charts and also provide a consolidated installation yaml files.


### Using Helm

Add helm repo for kyverno
```
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
```

Install kyverno in HA mode

```
helm install kyverno kyverno/kyverno -n kyverno --create-namespace --set replicaCount=3
```

or Install kyverno in the Standalone Mode

```
helm install kyverno kyverno/kyverno -n kyverno --create-namespace --set replicaCount=1
```

or Install a specific version of Kyverno

```
helm install kyverno kyverno/kyverno -n kyverno --create-namespace --version 2.6.5
```


### Using Kubernetes manifest yaml files 

```
kubectl create -f https://github.com/kyverno/kyverno/releases/download/v1.8.5/install.yaml
```

### Why Kyverno?

Kyverno is a policy engine designed for Kubernetes

A Kyverno policy is a collection of rules. Each rule consists of a match declaration, an optional exclude declaration, and one of a validate, mutate, generate, or verifyImages declaration. Each rule can contain only a single validate, mutate, generate, or verifyImages child declaration.

Policies can be defined as cluster-wide resources (using the kind ClusterPolicy) or namespaced resources (using the kind Policy.) As expected, namespaced policies will only apply to resources within the namespace in which they are defined while cluster-wide policies are applied to matching resources across all namespaces. Otherwise, there is no difference between the two types.

## Install ArgoCD

Threee ways to do so

```
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/install.yaml
```

Using Helm charts follow the [github-link](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd#installing-the-chart)

Using the ArgoCD OPerator use the the [link](https://argocd-operator.readthedocs.io/en/latest/install/olm/)



**Policies we are exploring around are here to follow** [link for policies](https://github.com/kyverno/policies/blob/main/best-practices/require-pod-requests-limits/require-pod-requests-limits.yaml)



## Some links to explore

[ArgoCD app setup](https://www.opsmx.com/blog/configuring-private-git-repo-in-argo-cd-to-deploy-kubernetes-apps/)

[How to run the ArgoCD Apps](https://blog.knoldus.com/how-to-create-applications-in-argocd/)
