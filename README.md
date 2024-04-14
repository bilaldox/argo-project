
# DevOps Internship Assignment - AI Planet

## Overview

Hello! My name is Mohammad Bilal Khan and this repo is specifically made for the DevOps Internship Assignment by AI Planet.

This repo contains the kubernetes and Argo manifestaions, including ArgoCD Application file and Argo Rollouts YAML file. Up next is the step-by-step walkthrough of how I implemented the GitOps deployment pipeline using ArgoCD and Argo Rollouts for advance deployment strategies, namely Canary release.
## Task 1/4: Setup and Configuration

I started with setting up a public repo on GitHub with title 'argo-project', cloned it on my local PC, and created the manifest files under the 'dev' directory, specifying the development enviroment.

Following that, I set up minikube on my local PC to get access to a kubernetes cluster. Inside that cluster, I installed ArgoCD and Argo Rollouts using kubectl commands. 
## Task 2/4: Creating the GitOps Pipeline

To deploy an application using GitOps pipeline, we first need an application. Then we convert it to a docker image and push it to a registry such as Docker Hub. 

For this project, I have used an already dockerized application provided by ArgoCD for their Rollouts demo in their documentation. 

These images are versionized by colors such as Blue, Yellow, Red etc. So each color is a different version of the application, such that each change in version/color of the application will start a new revision with canary deployment in the Argo Rollouts.

Link to these images (DockerHub):

[https://hub.docker.com/r/argoproj/rollouts-demo/tags](https://hub.docker.com/r/argoproj/rollouts-demo/tags)

After that, I created an application on ArgoCD, (I tried using both the UI and yaml file for practice). The yaml file for this app configuration can be found in this repo in the root directory named as 'argo-app-config.yaml'. 

Screenshot of the ArgoCD app:

![App Screenshot](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-14%2021-24-14.png?raw=true)

## Task 3/4: Implementing a Canary Release with Argo Rollouts

Then I configured the rollout.yaml file and applied it to the kubernetes cluster.

Screenshots of the rollout:

![Rollout Screenshot](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-14%2021-31-58.png?raw=true)
![Rollout Screenshot](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-14%2021-32-38.png?raw=true)

Here, the GitOps pipeline is fully set up. Now I change the version of the image from 'blue' to 'yellow' in the rollout.yaml file, and commit it.

We can see, as soon as the commit is made, the application sync the target state to match the desired state. In this case the target state is the Kubernetes cluster and the desired state is declared in the yaml files (manifests) in the github repo.

![Argo App](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-15%2001-43-45.png?raw=true)

To trigger the rollout canary release, I executed the following command in the git terminal:

```bash
  kubectl apply -f rollout.yaml
```
Once that is done, we can see the canary release in action. Both on UI and CLI:

![UI rollout](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-14%2022-37-37.png?raw=true)
![CLI rollout](https://github.com/bilaldox/argo-project/blob/main/images/Screenshot%20from%202024-04-14%2022-37-47.png?raw=true)


## Task 4/4: Clean up

To releas all the resources used in this assignment (argocd app & rollout), I executed the following commands:

```bash
kubectl delete -n argocd application web-app-gitops

kubectl delete -n argo-rollouts rollout web-app

```
And to remove the ArgoCD and Rollouts itself, we can remove their namespaces by executing the following commands:

```bash
kubectl delete namespace argocd

kubectl delete namespace argo-rollouts

```
## Conclusion

I would like to thank the AI Planet team for this opportunity. I was somewhat new to GitOps, and had never used ArgoCD before. I learned the technologies within whatever time I had, and I really learned a lot through this assignment. I enjoyed myself through the process, and will continue to build more upon it such as integrating it with CI and Observability tools.