# Lab 4 - Define a new task and test

In this section we will define and code a new task, based on the `Hello` example task.

## Type definition

First, we will define the name and properties of the task so Digital.ai Release can handle it.

We will do this in the file `type-definitions.yaml`.

💡 **Note:** If you have written plugins for Release before: this is a new format taking the place of `synthetic.xml`. 

You need to do the following:
1. Open the `type-definitions.yaml` file in the `resources` directory.
2. Rename `containerExamples.BaseTask` to `workshop.BaseTask`. Use Find & Replace!
3. Rename `containerExamples.Hello` to `workshop.Greet`
4. Remove `containerExamples.SetSystemMessage`, `containerExamples.ServerQuery` and `containerExamples.Server`

The result would be:

```yaml
types:
  workshop.BaseTask:
    extends: xlrelease.ContainerTask
    virtual: true

    hidden-properties:
      image:
        default: "@registry.url@/@registry.org@/@project.name@:@project.version@"
        transient: true
      iconLocation: test.png
      taskColor: "#667385"


  workshop.Greet:
    extends: workshop.BaseTask  # Don't forget to extend the new base task
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

With this metadata and other artifacts like icons, Release will be able to display the task in the UI and execute it. 
The build script packages this file together with the icon in the `jar` file that is uploaded to the Release server.

But before we do that, we need to get the code in shape!

## Update Python code

Based on `type-definitions.yaml`, the Python SDK will scan the `src` directory for Python classes with the same name as the type definition.

In the template project, there is a `containerExamples.Hello` task defined in `type-defintions.yaml` that has a corresponding `Hello` class in `src/hello.py`. Now that we have changed the task name to be `workshop.Greet`, there need to be a `Greet` Python class.

✍️ **Assignment**

Make everything consistent again by doing the following.

In the `src` directory,
1. Rename `hello.py` to `greet.py`
2. Inside `greet.py`, rename the `Hello` class to `Greet`
3. Remove the unused files `sample_release_api_task.py` and `sample_server_task.py`

💡 **Note for PyCharm users:** Ignore the warning about `sample_server_task.py` being used. We will fix that later when we take a look at the unit tests.

Now we are ready to build and test the plugin

✍️ **Assignment**

Repeat the steps from [Lab 1](lab-1-run-hello-world.md#build-integration-plugin-and-publish-the-container-image) to build and test the new plugin.

In summary:
1. Build using `sh build.sh --upload` or `build.bat --upload`
2. Refresh browser
3. Create a test template with the new task
4. Run it

This takes a bit, but it does validate if the integration plugin is built correctly and can be run inside Release. The debug/test loop can be shortened by using [Unit tests](#using-unit-tests), that we will cover at the end of this exercise.


## Code, build, test in a server

One feature worth highlighting is logging. Let's introduce an error in the code, by changing the greeting line to

```python
greeting = f"Hello {stranger}" 
```

Do the build and run the task again. It should fail!
You will get an error summary on the Activity page. 

![Show detailed logs](img/show-detailed-logs.png)

What's more, you can see the detailed logs by clicking on the **View logs** button

![Expand logs](img/expand-logs.png)

You can make the log area large by clicking on the **Expand** button.

## Using unit tests

It's pleasing to see your task running in Release. Less so if it fails... 

Luckily we can also run unit tests, catching most errors before we even install the integration plugin.

Unit tests are in the `test` directory. First let's modify the example tests that came with the template

✍️ **Assignment**

In the `tests` directory,
1. Rename `test_hello.py` to `test_greet.py` and inside the file, rename all references to 'Hello' to 'Greet'
2. Remove the `test_with_server.py` file

Run the unit tests from the top level directory of the repository with the command

    python -m unittest discover tests

If all went well, you should get the following error:

```
Unexpected error occurred.
Traceback (most recent call last):
  File "/Users/hsiemelink/Code/release-integration-template-python/venv/lib/python3.11/site-packages/digitalai/release/integration/base_task.py", line 29, in execute_task
    self.execute()
  File "/Users/hsiemelink/Code/hes-integration-workshop-python/src/greet.py", line 17, in execute
    greeting = f"Hello {stranger}"
                        ^^^^^^^^
NameError: name 'stranger' is not defined
E.
======================================================================
ERROR: test_hello (test_hello.TestHello.test_hello)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/hsiemelink/Code/hes-integration-workshop-python/tests/test_greet.py", line 21, in test_greet
    self.assertEqual(task.get_output_properties()['greeting'], 'Hello World')
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
KeyError: 'greeting'

----------------------------------------------------------------------
Ran 2 tests in 0.364s

FAILED (errors=1)
```

✍️ **Assignment** 
* Fix the code so the test runs without failure.  

## Do some coding!

Before we move on, try out some coding for yourself

✍️ **Assignment**
* Create another task in Python. Define a new type, with input and output variables. Keep it simple and just use standard Python libraries. We will touch more advanced topics in the next parts.

This concludes the first leg of our workshop. 
Congratulations! You are now able to build container-based plugins for Digital.ai Release! 

## Next

The rest of the workshop builds on the knowledge you have acquired so far. 

If you are more interested in coding, continue with
* [Part 2](../part-2/lab-5-create-a-third-party-integration.md) - How to integrate with a 3rd party system, using external Python libraries.

If you want to know how to prepare a Kubernetes cluster for a production set up of Digital.ai Release with containerized tasks, go to
* [Part 3](../part-3/lab-6-prepare-for-kubernetes.md) - Production Setup with Kubernetes, using the Remote Runner


