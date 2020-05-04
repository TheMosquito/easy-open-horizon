# easy-open-horizon

An easy way to publish stuff on an open-horizon exchange.

If you have an existing Docker container, and you wish to publish it on an open-horizon Exchange, and have open-horizon Agents deploy your container onto selected edge machines, then this is the easiest way to do that -- by far!

Of course, this simple procedure does not cover all possible features of open-horizon (e.g., user input variables) but it should enable you to run most Docker containers in open-horizon with very little effort.

## Information required

Before you begin, you need to collect these pieces of information:

#### PATTERN_NAME (a name of your choice for your open-horizon deployment Pattern)
#### SERVICE_NAME (a name of your choice for your open-horizon Service)
#### SERVICE_VERSION (a version number for your open-horizon Service -- NOTE: in must be in "SemVer" format, i.e., "N.N.N", e.g., "1.0.0")
#### SERVICE_CONTAINER (your full container ID, i.e., "registry/repo:version" -- If your container is in DockerHub, you can omit the "registry/" prefix)
#### CONTAINER_CREDS (your container access credentials, prefixed with "-r ", i.e., "registry/repo:user:token", e.g., "-r registry -- If you do not require credentials to access your container, set this to "")
#### ARCH (the hardware architecture of your edge machines -- NOTE: this must be the open-horizon architecture, which you can get on the edge machine by running`hzn architecture`)

Also, if your `docker run ...` command has any important arguments, you will need to know those too (see step 5 below).

## How to use this stuff

1. Install and configure the open-horizon Agent, and keep your creds in your shell environment as usual

2. Test the Agent installation and configuration with `hzn exchange user list` (if you get some nice JSON back, and no errors, then you are good to go)

3. Clone this repo and `cd` into the resulting directory

4. Edit the variables at the top of the `Makefile` using "Information required" from above

5. If you require any `docker run ...` configuration, add that to the `service.json` file. This example includes publication of container port 80 to host port 80, on all host interfaces. For details of how to specify other `docker run ...` configuration, see: https://github.com/open-horizon/anax/blob/master/doc/deployment_string.md

6. You should not need to touch the `pattern.json` file.

7. Run `make publish-service` to publish your container as an open-horizon Service

8. Run `make publish-pattern` to publish an open-horizon deployment Pattern that includes your published Service

9. On any edge machines where you want this pattern deployed, run `make register-pattern`

## Cleaning up

When you are done, just run `make clean`. That will:

- unregister the local edge machine
- remove from the open-horizon exchange the Service you had published
- remove from the open-horizon exchange the Pattern you had published
- remove the Docker container image from the local machine's Docker cache


