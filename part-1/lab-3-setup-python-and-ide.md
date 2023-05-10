# Lab 3 - Setting up your development environment

This section explains how you can set up Python with VisualCode or IntelliJ/PyCharm to help you code. 

## Installing Python

If you haven't already done so, you'll need to install **Python 3** on your computer. You can download the latest version of Python from the official website at https://www.python.org/downloads/.


### Virtual environment

We will create a **virtual environment** in Python, in order to keep all the dependencies of the plugin isolated. This will prevent interference with other Python projects on your computer.

Create a new Python virtual environment in the project directory by running the following command in your terminal:

    python -m venv venv

💡 **Note:** Some installations of Python use the `python3` command. In that case use `python3 -m venv venv`.   

This will create a new directory called `venv` in your project directory that will contain all the Python dependencies.

Activate the virtual environment by running the following command

**Windows:**

    venv\Scripts\activate

**Linux / macOS:**

    source venv/bin/activate

Once activated, you should see the name of the virtual environment displayed in your command prompt or terminal, for example `(venv) C:\Users\username\project` on Windows.

Te exit the virtual environment, simply use the `deactivate` command in the terminal.

### Install libraries required by the project

Install the required Python libraries using the following command:

    pip install -r requirements.txt 

This command will install the required Python packages listed in the `requirements.txt` file within the virtual environment.

Now you have a virtual environment set up with the required dependencies needed for the project.

## IDE support

### PyCharm

You can download the latest version of PyCharm from the official website at https://www.jetbrains.com/pycharm/download/. 

Open PyCharm and select **Open** from the welcome screen. Navigate to the directory where you cloned the project and open it.

Then it should work out-of-the-box. PyCharm will even create a `venv` Python virtual environment for you, so you can skip the previous steps.

### Intellij

Intellij as PyCharm's older brother and works similar to PyCharm. However, there seems to be a bit more manual setup.

IntelliJ needs to know which version of Python to use for your project. 
* Go to **File > Project Structure > Project Settings > Project
* Select **Add Python SDK** from the **SDK** dropdown
* Choose **Existing environment" and select the `...` icon on the right. Navigate to the virtual environment you just created, then select `bin/python3`

You are now good to go to start coding!

### Visual Code

Follow the instructions from the [Visual Code Python tutorial](https://code.visualstudio.com/docs/python/python-tutorial) to install the Python plugin for Visual Code.


---
[Next](lab-4-define-a-new-task-and-test.md)



