# easy-open-horizon

This project is glue code that I use for publishing containerized applications with Open Horizon. I wrote this because I have difficulty following the [official Open Horizon documentation](https://github.com/open-horizon/examples/tree/master/edge/services/helloworld). I hope you will find this helpful too, but if you run into troubles please go to the official documentation and follow the procedures there.

If you have an existing Docker container, and you wish to publish it on an Open Horizon Exchange to have Open Horizon Agents deploy your container onto selected edge machines, then this is the easiest way to do that -- by far!

Of course, this simple procedure does not cover all possible features of Open Horizon (e.g., user input variables) but it should enable you to run most Docker containers in Open Horizon with very little effort.

## Information required

Before you begin, you need to collect these pieces of information:

#### PATTERN_NAME
a name of your choice for your Open Horizon deployment Pattern
#### SERVICE_NAME
a name of your choice for your Open Horizon Service
#### SERVICE_VERSION
a version number for your Open Horizon Service -- **NOTE:** in must be in "SemVer" format, i.e., "N.N.N", e.g., `1.0.0`
#### SERVICE_CONTAINER
your full container ID, i.e., "registry/repo:version" -- If your container is in DockerHub, you can omit the `registry/` prefix
#### CONTAINER_CREDS
your container access credentials, prefixed with "-r ", i.e., "-r registry/repo:user:token", e.g., `-r "registry.wherever.com:myid:mypw"` -- If you do not require credentials to access your container, set this to an empty value (no quotes, and no `-r ` prefix)
#### ARCH
the hardware architecture of your edge machines -- **NOTE:** this must be the Open Horizon architecture, which you can get on the edge machine by running`hzn architecture`

#### Arguments to `docker run`
If your `docker run ...` command has any important arguments, you will need to know those too (see step 5 below).

## How to use this stuff

1. Install and configure the Open Horizon Agent, and keep your creds in your shell environment as usual

2. Test the Agent installation and configuration with `hzn exchange user list` (if you get some nice JSON back, and no errors, then you are good to go)

3. Clone this repo and `cd` into the resulting directory

4. Edit the variables at the top of the `Makefile` using the "Information required" from above

5. If you require any `docker run ...` configuration, add that to the `service.json` file. This example includes publication of container port 80 to host port 80, on all host interfaces. For details of how to specify other `docker run ...` configuration, see: https://github.com/open-horizon/anax/blob/master/doc/deployment_string.md

6. You should not need to touch the `pattern.json` file.

7. Create a cryptographic code signing key pair: `hzn key create <yourcompany> <youremail>`

8. Run `make publish-service` to publish your container as an Open Horizon Service

9. Run `make publish-pattern` to publish an Open Horizon deployment Pattern that includes your published Service

10. On any edge machines where you want this pattern deployed, run `make register-pattern`

## Cleaning up

When you are done, just run `make clean`. That will:

- unregister the local edge machine
- remove from the Open Horizon exchange the Service you had published
- remove from the Open Horizon exchange the Pattern you had published
- remove the Docker container image from the local machine's Docker cache

