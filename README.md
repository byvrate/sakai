

# sakai-docker

Documentation and steps to setup Sakai in Docker for ByVrate. The current motivation for this is simply testing and sandbox. 

A docker image was created from the docker file since no pre-existing images exist in DockerHub. Therefore you must build the image before you can use Docker compose. After running the setup, you should be able to run Sakai in Docker Desktop after everything is built.

> Note: A [gitmodule](https://git-scm.com/docs/gitmodules) is used to pull down Sakai source.

## Prerequisites

The following steps in this section aren't needed to run Sakai in Docker and are only for documentation purposes.

First add a submodule using the git command.

```sh
git submodule add https://github.com/sakaiproject/sakai.git
```

This will add a *.gitmodules* files in the root of the repo. It will pull down all source from the master branch. However, we don't want that version. We want to target version 12.5. The [Releases](https://github.com/sakaiproject/sakai/releases) page shows version 12.5 is on commit [318425357a46fe0e87e840fe9080b648417a3a6b](https://github.com/sakaiproject/sakai/commit/318425357a46fe0e87e840fe9080b648417a3a6b). Thefore, after the submodule was added, a checkout was made to target the 12.5 tag whis is the same as the commit. 

```sh
cd sakai
git checkout 12.5
cd ..
git add sakai
git commit -m "moved sakai submodule to version 12.5"
git push
```

## Setup

Install [Docker Desktop](https://www.docker.com/products/docker-desktop)

Navigate to tools/sakai so this is your current working directory. All commands should be run from this directory. 

Run a Git submodule update to fetch/update the source code for Sakai.

```sh
git submodule update --init
```

> ***Important:*** In the sakai sub module (sakai git repo), navigate to tools/sakai/sakai/master/pom.xml and change all occurances of *http://repo1.maven.org/maven2* to *https://repo1.maven.org/maven2*

Build the Docker image from source. This will create an image locally on your desktop.

```sh
docker build -t byvrate/sakai:12.5 .
```

After it builds succesfully, you should have the image on your machine. Make sure to verify this.
```sh
docker image ls
```

Now let's run Sakai by using Docker Compose

```sh
docker-compose up
```


## Logging into Sakai

Once the Sakai container image has been built and the container is created,
you can access Sakai by browsing to http://localhost:8080/portal.

The default admin user is `admin` and the password is `admin`.

## Troubleshooting

If you have problems running or building Docker, you may need to bump up the RAM to at least 3GB. 

This can be done in the "Advanced" section of Docker for Mac's Preferences UI.

Make sure not to store too much information. There are no volumes attached to the mysql container.

If you get build errors while running the docker file, you may have missed some the search and replace. See above. 

If docker compose can't find image *dli/sakai:12.5* then it's not on your machine. Make sure the build worked.

## References

[Quick Start from Source](https://github.com/sakaiproject/sakai/wiki/Quick-Start-from-Source)

https://github.com/hypothesis/sakai-docker

