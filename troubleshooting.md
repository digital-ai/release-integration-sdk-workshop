### Troubleshooting

If Release won't start up with the following message:

```
java.lang.IllegalStateException: Trying to register duplicate definition for type (...)
```

or

```
java.lang.NullPointerException: Could not find a type definition associated with type [containerExamples.BaseTask]
```

The easiest is to reset the dev environment by doing

    docker compose down
    docker compose up -d --build

Unfortunately you will lose your work

XXX Rewrite / expand


