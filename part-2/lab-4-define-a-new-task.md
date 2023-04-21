XXX Write this section based on the notes

Edit `type-defintions.yaml`

```yaml
types:
  hes.BaseTask:
    extends: xlrelease.ContainerTask
    virtual: true

    hidden-properties:
      image:
        default: "@registry.url@/@registry.org@/@project.name@:@project.version@"
        transient: true
      iconLocation: test.png
      taskColor: "#667385"


  hes.Greet:
    extends: containerExamples.BaseTask
    description: "Simple greeter task"

    input-properties:
      yourName:
        description: The name to greet
        kind: string
        default: World

    output-properties:
      greeting:
        kind: string
```

* rename hes.BaseTask, hes.Greet
* Change extends: hes.BaseTask
* Remove dummy_json.py and sets_system_messgae.py
* Rename hello.py to greet.py
* Rename Hello class to Greet class
* Install in Release & restart
* Make a small change in the code
* Then build but do not restart and see that changes will be picked up
* Introduce an error, build and run and show how to see the logs

