# Exploring Tanzu Build Services and re-building my Pac-Man Demo Container

## Introduction

One of the ideas I've had in the back of my head is looking at how to repackage the Pac-Man Container I use, and fix some of the vulnerabilities. 

Speaking with a colleague they mentioned I could look at using Tanzu Build Services, to easily generate new containers from my code. I've know of the product for a while, but never really look at it in any anger. So I decided to have a look at it.
## Prerequisite

You will need a supported Kubernetes Cluster. See this page for more details.
- [Getting Started with Tanzu Build Service](https://docs.vmware.com/en/Tanzu-Build-Service/1.3/vmware-tanzu-build-service-v13/GUID-getting-started.html#install-rbac)
## Use Case

````
Tanzu Build Service uses the open-source Cloud Native Buildpacks project to turn application source code into container images. Build Service executes reproducible builds that align with modern container standards, and additionally keeps image resources up-to-date. It does so by leveraging Kubernetes infrastructure with kpack, a Cloud Native Buildpacks Platform, to orchestrate the image lifecycle. The kpack CLI tool, kp can aid in managing kpack resources.

Build Service helps you develop and automate containerized software workflows securely and at scale.
````
## Try yourself

Here is the blog post I wrote up of how I deployed this into my demo lab and pushed my Pac-Man Dockerfile through the service to generate a new container. 

- [First Look – Setup Tanzu Build Services and rebuilding Pac-Man](https://veducate.co.uk/tanzu-build-services-rebuilding-pac-man/)