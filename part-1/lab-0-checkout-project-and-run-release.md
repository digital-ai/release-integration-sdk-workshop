
# Lab 0 - Checkout template project and run Digital.ai Release

## Collect all prerequisites

Check all prerequisites:

* A git client
* A GitHub account
* Docker Desktop

Start Docker.

## Checking out the template project

The Digital.ai Release Integration SDK provides a template project to get started. 

The template project **release-integration-template-python** is located on GitHub:

https://github.com/digital-ai/release-integration-template-python

In this lab we will use the template project directly to make sure you can run Digital.ai Release and run a sample plugin. To build a custom integration, you would create a duplicate of this project. During the workshop, we will do this in Lab 2.

Create a copy of this project on your local machine with the following command

    git clone git@github.com:digital-ai/release-integration-template-python.git

## Run a development instance of Digital.ai Release

Open a terminal window and launch the development environment with the following commands

    cd release-integration-template-python
    cd dev-environment
    docker compose up -d --build

When the docker command is finished, the containers will still be starting. Check the logs of the `digitalai-release-1` container until it displays the following lines:

    Digital.ai Release has started.
    You can now point your browser to http://host.docker.internal:5516/

In a browser, go to http://localhost:5516. This should take you to the Digital.ai Release login screen. Log in with username `admin` and password `admin`


---

[Next](lab-1-run-hello-world.md)

