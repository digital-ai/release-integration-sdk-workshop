![Build new integrations in an instant!](integration-sdk-logo.png)

To make your job easier, we are bringing a new integration SDK experience that gets you up and running in a matter of minutes.

Run tasks as containers, using any language or third-party library.

## SDK Workshop

In this workshop, you will learn how to

* Build and maintain custom integrations using Python 3
* Set up a development environment for building, testing, installing, and running integration tasks
* Configure a production-style architecture to run the container-based tasks in a Kubernetes cluster

## Prerequisites

You will run this workshop on your own machine. You will need to have the following installed

### Bare minimum

To run the HelloWorld example and do a few minor modifications to the example, you only need

* A git client
* Docker
* Admin privileges on your system

This is all to get started!

The template project to get you started provides build scripts and a development environment that will launch Digital.ai Release with a temporary license.

### Development exercises

To start developing the integrations plugins, install the following

* Python3, version 3.11
* The `pip` installer tool for Python
* A code editor or IDE like VisualCode or PyCharm

### Production setup (optional)

Container-based integration plugins are meant to run on a Kubernetes cluster. To have a hands-on experience you will need the following components.

🐲 _**Note:**_ Some Kubernetes experience is needed to make the best of this exercise. Feel free to skip this part of Kubernetes is all new to you (or be prepared for a steep learning curve)

Refer to [Part 4](part-4/lab-6-prepare-for-kubernetes.md) for a detailed list of requirements regarding the Kubernetes setup.

## Workshop Contents

### Introduction
* Presentation sketching out the architecture and what we are going to do



### [Part 0 - Run Release](part-0/)

* [Lab 0 - Check out and run](part-0/lab-0-checkout-project-and-run-release.md)

### [Part 1 - Hello World](part-1/)

*  [Lab 1 - Run Hello World](part-1/lab-1-run-hello-world.md)

### [Part 2 - Create a custom task](part-2/)

* [Lab 2 - Create a project repository](part-2/lab-2-create-project-repository.md)
   * Create a project repository in Git
   * Build and install Hello example
   * Wipe environment (docker compose down and up)
* [Lab 3 - Define a new task]()
   * Change type-definitions
   * Create a new task in Python
   * Build & install
   * Run it
   * As code?

### [Part 3 - Create an integration task](part-3/)

* [Lab 4 - Set up IDE]()
   * Install Python
   * Create virtual environment
   * pip install libraries
   * Load in IDE

* [Lab 5 - Create integration to a third party server]()
   * Download external library. Splunk SDK? We can run it is a container
   * Define server type in type-modifications
   * Define task
   * Build and run

### [Part 4 - Production Setup with Kubernetes](part-4/)

* [Lab 6 - Prepare Kubernetes](part-4/lab-6-prepare-for-kubernetes.md)
   * Launch Kubernetes cluster
* [Lab 7 - Install Remote Runner]()
   * Create user and token in Release
   * Install Remote runner with xl kube install
   * Check that Remote Runner is being registered in Release
   * Disable Docker runner
   * Run a task
   * Check in K9s


---

[Start](./part-0/lab-0-checkout-project-and-run-release.md)
