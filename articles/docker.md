---
title: Synapse Docker Registry
layout: article
excerpt: The Synapse Docker registry provides a space for Synapse users to store and distribute their Docker images per Synapse project.
category: howto
---

Docker is a tool for creating, running, and managing lightweight virtual machines. These virtual machines make it possible to distribute executable environments with all of the dependencies that can easily be run by others. These Docker images can then be stored and distributed on a Docker registry, a collection of these images. There are a number of open registries on the web, and Synapse hosts a private registry, freely available to our users, which will allow users to create software on a per project basis which can be easily shared across Synapse. Learn more about [Docker](https://www.docker.com/products/overview) and [Docker registry](https://www.docker.com/products/docker-registry).

Synapse users interact with the Synapse Docker registry using the standard Docker client. In Synapse, Docker containers are represented as versioned 'repositories' under the 'Docker' tab. As with Files and Tables, Repositories are organized by project and inherit the access controls from the project.

## Creating a new Docker image

Let's begin by creating a custom Docker image.  Users can choose to either modify an existing Docker image or build a Docker image from a Dockerfile.  Docker images must be tagged with 'docker.synapse.org/synapseProjectId/myreponame' to allow images to be saved.

**Tagging an existing Docker image to save onto the Synapse registry**

``` console
docker pull ubuntu
```

To tag an existing Docker image, users can use the IMAGE ID or the repo name.  The IMAGE ID can be found by doing:

``` console
docker images
#REPOSITORY	TAG	IMAGE ID	CREATED	SIZE
#ubuntu	latest	f8d79ba03c00	6 days ago	126.4 MB
```

Tag the Docker image:

``` console
docker tag f8d79ba03c00 docker.synapse.org/syn12345/mytestrepo:version1
#or
docker tag ubuntu:latest docker.synapse.org/syn12345/mytestrepo:version1
#syntax: docker.synapse.org/<projectId>/<repoName>:<tag>
```

You can also choose to not tag your image with an explicit tag, which will by default tag it with `latest`.

``` console
docker tag f8d79ba03c00 docker.synapse.org/syn12345/mytestrepo
```

**Build your own image from a Dockerfile**
When building a Docker image from a Dockerfile, add a `-t` to the docker build command with the correct Synapse Docker registry tag.

``` console
docker build -t  docker.synapse.org/syn12345/my-repo path/to/dockerfile
```

Learn more about building [docker images](https://docs.docker.com/engine/getstarted/step_four/).  

## Storing Docker images in the Synapse Docker Registry

To store Docker images, use the `docker push` command.  To push to the Synapse Docker Registry, users must be logged into the registry:

``` console
docker login -u <synapse username> -p <synapse password> docker.synapse.org
```

After logging in, view your images and decide which ones to push into the registry.

``` console
docker images
#REPOSITORY                                 TAG                 IMAGE ID            CREATED             SIZE
#docker.synapse.org/syn12345/mytestrepo   version1            f8d79ba03c00        6 days ago          126.4 MB
#ubuntu                                     latest              f8d79ba03c00        6 days ago          126.4 MB
#docker.synapse.org/syn12345/my-repo	latest	df323sdf123d	2 days ago	200.3 MB
docker push docker.synapse.org/syn12345/mytestrepo:version1
docker push docker.synapse.org/syn12345/my-repo
```

## Using Docker images stored in the Synapse Docker Registry

To access the Docker images stored in Synapse, use the `docker pull` command.

{% include tip.html content="By default, if you do not specify a tag, it will attach latest as the tag.  If you specified a tag on your repository, be sure to pull the repository with the tag." %}

``` console
docker pull docker.synapse.org/syn12345/my-repo
```

Docker tags can be assigned to later commits. If you want to be explicit about the version of an image then instead of referencing a tag you can reference a digest:

``` console
docker pull docker.synapse.org/syn12345/mytestrepo@sha256:2e36829f986351042e28242ae386913645a7b41b25844fb39b29af0bdf8dcb63
```

where the digest for a commit is printed to the command line after a successful Docker push. The Synapse web portal displays current digests for a repository's tags on the repository's Synapse page.

{% include note.html content="You can add external repositories, i.e. repositories that live in other registries like DockerHub and quay.io. For these repositories there is no tight integration (Synapse doesn't contact these external registries) but it allows you to list Docker repositories that are relevant to the project but are not within Synapse.
" %}

## Debugging Issues

Unfortunately, while using the Docker client with the Synapse Docker registry, you may encounter an `unauthorized: authentication required` error. This can be caused by an erroneous user name or password or other reasons. To differentiate between the two a user can type:

```
docker login docker.synapse.org
```

Once login succeeds, the correct credentials will be cached on the machine, so any further issues can be attributed to sharing settings or trashing of the repo, as described below.

* To pull a Docker repo' you must have download access in the sharing settings of the parent project.
* To push a Docker repo' you must be a Synapse certified user and have create permission if the repo does not exist or update permission if it does.
* Any operation will fail if the repo is in the trash can.
Note: In Synapse, Docker repo's have names of the form: docker.synapse.org/syn123456/path/to/repo, where syn123456 is the project ID, so it should be clear which project's sharing settings to check.
