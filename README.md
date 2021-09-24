This repo showcases a working end-to-end pipeline. Not uploaded files that need to be included are:

git-credentials.yaml --> pipeline/base:

apiVersion: v1
kind: Secret
metadata:
  name: git-credentials
type: kubernetes.io/basic-auth
stringData:
  username: git-username
  password: personal-access-token



to deploy the e2e-pipeline successfuly, the following steps are needed to be executed in pipeline/base:

- creation of a robot account on quay.io, with R/W permissions
- downloading a quay.io secret which is applied to the cluster/namespace we are in
- adjust the secret's name of the build-bot.yaml to the name of the secret that has just been applied
- update the triggertemplate with the (working) pipelinerun-file
- creation of a webhook: create a github-webhook following this tutorial (important: no specificaton of the port needed, use the https address pointing to the trigger (see oc topology, command: oc console)):https://developer.ibm.com/tutorials/github-webhook-triggers-openshift/


these changes need to be made to k8s:

1. set Image value in the command line:
retrieve _your-sha_ from quay.io and the respeciive project where the image has been pushed to.


IMAGE_SHA="_your-sha_"
kustomize edit set image quay.io/<path-to-repository>:${IMAGE_SHA}

2. Next, set the labels (again from command line):

kustomize edit set label "app:<your-name>-express-sample-app"
kustomize edit set label "app.kubernetes.io/instance:<your-name>-express-sample-app-instance"
kustomize edit set label "app.kubernetes.io/name:<your-name>-express-sample-app"


To use ArgoCD:

login to argocd and create a new project.

applicaton name: the name given to the app (defined in pipelinerun)

Project: Default, Sync Policy: Automatic

All boxes checked

source is the https of the github repo

path: path within the repo pointing to the manifest.yaml (without file name being specified)

application name is the namespace of the cluster we are logged into (aka the project)

the course's contents is published here:

https://github.com/cloud-native-garage-method-cohort/emea-5-ci-cd-from-first-principles/blob/main/SCHEDULE-EMEA.md




a brief explanation of all tools that have been used:

Docker: 

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

quay.io:

Similar to code, developers typically share Docker images, which are then run as containers. The location that we will store our Docker images is Quay.io.

kubernetes:

Kubernetes is a platform of platforms. At its core, Kubernetes is "a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation." 

"Containerized workloads" sounds like something potentially related to Docker. "Declarative configuration" sounds like it means that we would interact primarily with text files where we declare the configuration we want. "Automation" sounds like things will be done for us. We will see if this intuition is correct or not, given this difficult-to-understand definition.

Tekton:

Tekton is a cloud native continuous integration and delivery (CI/CD) solution. It allows developers to build, test, and deploy across cloud providers and on-premise systems.
Tekton is built on Kubernetes as a set of Custom Resource Defintions. Tekton is a newer ecosystem that is rapidly evolving and has a bold mission statement:
Be the industry-standard, cloud-native CI/CD platform components and ecosystem.

Buildah: 

buildah is tool for building Open Container Initiative (OCI) containers. buildah, for your case, will use a Dockerfile to build a container.

Kustomize:

Kustomize is a tool for customizing Kubernetes YAML configuration files.

ArgoCD: 

In other words, after an uphill battle with Continuous Integration, Continuous Delivery - due to the GitOps Pull-based model - becomes nothing more than "letting the tools do their job." In other words, GitOps' Pull Based model says that an entity should watch the repository for changes, then apply them to the environment automatically which handles consistency itself. ArgoCD makes this simple to use and high leverage as a tool. Like the other tools covered in this course, ArgoCD is new and still rapidly developing.


This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.js`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.js`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.
