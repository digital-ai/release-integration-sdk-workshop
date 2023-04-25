# Troubleshooting

## Release won't start

Release complains on startup and quits.

For example with the following message:

```
java.lang.IllegalStateException: Trying to register duplicate definition for type (...)
```

or

```
java.lang.NullPointerException: Could not find a type definition associated with type [containerExamples.BaseTask]
```

This is usually caused by a plugin with incorrect an `type-definitions.yaml` file

The easiest way is to simply reset the dev environment by doing

    docker compose down
    docker compose up -d --build

Unfortunately you will lose your work!

(We are currently working on ways to prevent this situation)

## My task doesn't show up in the Add task menu

* Refresh your browser

## My task shows up but properties are missing

* Refresh your browser

## (M1 Mac) qemu: uncaught target signal 11 (Segmentation fault) - core dumped

Happens when starting digitalai-release-setup container. So far only seen on M1

* Workaround is to configure the Release server with the command 

  xl apply -f dev-environment/digitalai-release-setup/instance-configuration.yaml

Install the xl command utility for this -- see also [Lab 6](part-3/lab-6-prepare-for-kubernetes.md#set-up-the-xl-client)


