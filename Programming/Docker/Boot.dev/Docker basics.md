To put it simply: Docker allows us to deploy our applications inside "containers" which can be thought of as _very_ lightweight virtual machines.

- A **"docker image"** is the read-only _definition_ of a container
- A **"docker container"** is a virtualized read-write environment

Many docker containers are "stateless", or at least stateless in the persistent sense. That is, when you create a new container from the same image, it won't store any information from before. This is solved by using **volumes.**

You can force a container into offline mode! [[Offline mode]]

Docker allows us to create custom [bridge](https://docs.docker.com/network/bridge/) networks so that our containers can communicate with each other if we want them to, but remain otherwise isolated. 

### Dockerfiles
Dockerfiles are _amazing_ because they allow us to define the environment our applications are meant to use _in code_. We can even commit the Dockerfiles to Git alongside our source code. [[Example Dockerfile python]]

### Benefits of dockerizing
You may be thinking, "What's the point of dockerizing?

- Anyone with Docker can run your image, regardless of their OS
- You _could_ deploy your image on any cloud service that uses images (most of them). [Kubernetes](https://kubernetes.io/), which is something we'll cover later, is a way to build entire production back-end architectures using dockerized apps.
- If your server is written in Python, you can bundle the required interpreter inside of your image. Users won't need to download and configure their own Python versions.

### Docker Hub
[Docker Hub](https://hub.docker.com/) is the official cloud service for storing and sharing Docker images. We call these kinds of services "registries". Other popular image registries include:

- [AWS ECR](https://aws.amazon.com/ecr/)
- [GCP Container Registry](https://cloud.google.com/container-registry)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Harbor](https://goharbor.io/)
- [Azure ACR](https://azure.microsoft.com/en-au/products/container-registry/)

### Use semantic versioning for dockerfiles
Use [semantic versioning](https://semver.org/) on all your images, but to _also_ push to the "latest" tag. That way you can keep all of your old versions around, but the `latest` tag still always points to the latest version.



