Docker Notes

1. Container: think them as very lightweight wrappers around a single Unix process. Containers are lightweight. The new container is so small because it is just a reference to a layered filesystem image and some metadata about the configuration.

Containerized processes are also just processes on the Docker server itself. They are running on the same exact instance of the Linux kernel as the host operating system. They even show up in the ps output on the Docker server. 

2. Revision Control: Containerized processes are also just processes on the Docker server itself. They are running on the same exact instance of the Linux kernel as the host operating system. They even show up in the ps output on the Docker server. 

3. Docker filesystem layers: Docker containers are made up of stacked filesystem layers, each identified by a unique hash, where each new set of changes made during the build process is laid on top of the previous changes. That’s great because it means that when you do a new build, you only have to rebuild the layers that include and build upon the change you’re deploying. To simplify this a bit, remember that a Docker image contains everything required to run your application. Instead, Docker will use as many base layers as it can so that only the layers affected by the code change are rebuilt.

4. Docker Image Tags: it provides image tagging at deployment time so you know what was the previous version. You can leave multiple revisions of your application on the server and just tag them at release. 

In many examples on the Internet and in this book, you will see people use the latest tag. This is useful when getting started and when writing examples, as it will always grab the most recent ver‐ sion of a image. But since this is a floating tag, it is a bad idea to use latest in most workflows, as your dependencies can get updated out from under you, and it is impossible to roll back to latest because the old version is no longer the one tagged latest.

5. Dokcer Building: The Docker command-line tool contains a build flag that will consume a Dockerfile and produce a Docker image. Each command in a Dockerfile generates a new layer in the image, so it’s easy to reason about what the build is going to do by looking at the Dockerfile itself. The great part of all of this standardization is that any engineer who has worked with a Dockerfile can dive right in and modify the build of any other application. Because the Docker image is a standardized artifact, all of the tooling behind the build will be the same regardless of the language being used, the OS distri‐ bution it’s based on, or the number of layers needed.

6. Docker client
The docker command used to control most of the Docker workflow and talk to remote Docker servers.

7. Docker server
The docker command run in daemon mode. This turns a Linux system into a Docker server that can have containers deployed, launched, and torn down via a remote client.

8. Docker images
Docker images consist of one or more filesystem layers and some important met‐ adata that represent all the files required to run a Dockerized application. A sin‐ gle Docker image can be copied to numerous hosts. A container will typically have both a name and a tag. The tag is generally used to identify a particular release of an image.

9. Docker container
A Docker container is a Linux container that has been instantiated from a Docker image. A specific container can only exist once; however, you can easily create multiple containers from the same image.

10. Atomic host
An atomic host is a small, finely tuned operating system image, like CoreOS and Project Atomic, that supports container hosting and atomic OS upgrades.


