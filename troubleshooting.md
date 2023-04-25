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


