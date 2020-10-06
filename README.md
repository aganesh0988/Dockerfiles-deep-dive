# Dockerfiles Deep Dive

A Dockerfile is a text file that is used to create a Docker image. In other 
words, a Dockerfile defines the custom environment used in a Docker container. 
The Dockerfile contains a list of instructions that Docker will execute from a
`docker build` command.

You’ll want to create your own Dockerfile any time existing images don’t satisfy
your project needs. This will happen most of the time, which means that learning
how to write a Dockerfile is a pretty essential part of working with Docker.

In today's project, you will build a custom Dockerfile for each phase and create
an image from each Dockerfile. Before you finish, you will add different 
environment variables, networks, and volumes.

## The approach

Your workflow will be as follows:

1. Create the Dockerfile and define the steps that build up your images
2. Issue the `docker build` command to build a Docker image from your Dockerfile
3. Use this image to start containers with the `docker container run` command
4. Iterate and rebuild as needed

Because the `docker build` command caches an intermediate image after each
instruction, best practice says to structure your Dockerfile from least-often
changed at the beginning to most-often changed at the end.

Generally, this means

1. Install tools that are needed to build your application.
2. Install dependencies, libraries and packages.
3. Build your application.
4. Start your application.

### Your mission

The only file you will need to edit in each phase is the __Dockerfile__.
Additional edits are purely at your discretion.

## Before you begin

Prepare yourself to work through a number of **Docker** commands and 
`Dockerfile` instructions using documentation, including cleaning up after 
yourself when you make mistakes (we all do!). You will be provided with starter
files including a working application for each phase. This will allow you to 
focus on the Dockerfile in each phase of this project.

### Help and hints

If you are need help with Dockerfile commands the Dockerfile
[documentation][docker-docs] is your best friend!

**Remember**: Each command you use in a Dockerfile(`FROM`, `RUN`, `WORKDIR`,
`COPY`, `EXPOSE`, and `CMD`) will create a new layer. You can string together 
commands by utilizing `\` at the end of one line to signal that it continues
on the next line. You can also chain commands using the `&&` symbol, so that 
later commands won't run unless each earlier ones is successful.

Here is an example of both being used:

```docker
 RUN apt-get update \
    && apt-get install --no-install-recommends
```

### Remember to clean up after yourself

Before you start, make sure to stop and remove any containers you have running,
so they don't interfere with your testing.

Check on what's running using `ls` or `ps` commands like these.

```bash 
docker ps -a
docker volume ls
docker network ls
```

Your goal is to get back to this state:

* No containers
* No volumes
* Three networks (the defaults of `bridge`, `host` and `none`)

Clean up as needed with a combination of `stop` and `prune` commands.

```bash 
docker container stop $(docker ps -a -q)
docker container prune -f
docker volume prune -f
docker network prune -f
```

## Phase 1: Static website (HTML served through NGINX)

As you saw in previous lessons, the [`nginx` image on Docker Hub][image-nginx] 
comes with all the instructions to host a website, including a default 
index.html file. Your goal in this phase is to replace the default 
__index.html__ with a custom one (from the starter or of your own making).

Start the `Dockerfile` as most **Dockerfiles** will start, using the `FROM` 
instruction. To get the simplest version of nginx (so your image is as small as
possible), it is recommended that you use the latest version on `alpine`.

You'll need to make sure you are copying your html into the correct folder.
The default root directory for NGINX on Alpine Linux is `/usr/share/nginx/html`.
In your Docker file, add an instructions to change the working directory to 
this path.

The final instruction is to copy in your __index.html__ file. Add that to your
Dockerfile now.

> Important: The NGINX base image already includes the CMD that will start the
> web server so you do not need to repeat that in your __Dockerfile__.

Finally, you are ready to build the image! Remember to use a tag; the typical
format of a tag is `<username/imagename>`.

```bash 
docker build -t abc/deep-dive-phase-1 .
```

To confirm your image is working correctly, you'll need to run a container from
that image with the proper port. Remember to name your container. Optionally,
you can run in detached mode.

```bash 
docker container run -p 8080:80 --name deep1 -d abc/deep-dive-phase-1
```

In your browser, go to [http://localhost:8080][local-nginx-url]. If you see 
the one-page website, then success!

### Save your progress

As you work on projects, you should commit your Dockerfile with all your other 
project files.

> Important: Take a moment right now to use Github and commit these files to get
> those green squares!

Clean up before the next phase by stopping the container and removing it 
([see above for commands](#before-you-begin), if needed). Now, you are ready to
move on to phase 2!

## Phase 2: Express app

As you know, **Express** apps run through the **node** engine, so begin by 
looking for the most appropriate [`node` image on Docker Hub][image-node].

...

...

... Tip: In the end you should be using these commands (in the appropriate order):
CMD, COPY, EXPOSE, FROM, RUN, WORKDIR ...

... Load and install packages before applications files for better caching
since the packages change less frequently than code files ...

Finally, you are ready to build the image! Remember to use a tag; the typical
format of a tag is `<username/imagename>`.

```bash 
docker build -t abc/deep-dive-phase-1 .
```

To confirm your image is working correctly, you'll need to run a container from
that image with the proper port. Remember to name your container. Optionally,
you can run in detached mode.

```bash 
docker container run -p 8080:80 --name deep1 -d abc/deep-dive-phase-1
```

In your browser, go to [http://localhost:8081][local-express-url]. If 
**Express** says "Hello", then you successfully created another Dockerfile!

### Save your progress

...

## Phase 3: React app

...

## Phase 4: Python app

...


## Bonus Phase A: Health Checks

Prove you are testing oriented by writing [health checks][health] for all the
`Dockerfiles` you've written today. After you've finished, make sure your images
all run properly before you re-push your changed images up to Docker Hub. Moby
Dock would be proud! 🐳

## Bonus Phase B: Kubernetes Tutorial

Follow this awesome [Kubernetes Tutorial][kubernetes] to start learning about
one of the most supported orchestration platforms. Kubernetes is a **very**
popular choice for companies looking to deploy containerized applications onto a
cluster. Be the best developer you can be by learning about how to scale
deployments!


[docker-docs]: https://docs.docker.com/engine/reference/builder/
[image-nginx]: https://hub.docker.com/_/nginx
[local-nginx-url]: http://localhost:8080

[image-node]: https://hub.docker.com/_/node
[local-express-url]: http://localhost:8081/

[health]: https://docs.docker.com/engine/reference/builder/#healthcheck
[kubernetes]: https://kubernetes.io/docs/tutorials/kubernetes-basics/
