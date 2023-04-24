# Lab 3 - Setting up your development environment

This section explains how you can set up Python with VisualCode or IntelliJ/PyCharm to help you code. 

## Installing Python

If you haven't already done so, you'll need to install **Python 3** on your computer. You can download the latest version of Python from the official website at https://www.python.org/downloads/.

XXX Check Docker image


### Virtual environment

We will create a **virtual environment** in Python, in order to keep all the dependencies of the plugin isolated. This will prevent interference with other Python projects on your computer.

Create a new Python virtual environment in the project directory by running the following command in your terminal:

    python3 -m venv venv

This will create a new directory called `venv` in your project directory that will contain all the Python dependencies.

Activate the virtual environment by running the following command

**Windows:**

    venv\Scripts\activate

**Linux / macOS:**

    source venv/bin/activate

Once activated, you should see the name of the virtual environment displayed in your command prompt or terminal, for example `(venv) C:\Users\username\project` on Windows.

Te exit the virtual environment, simply use the `deactivate` command in the terminal.

### Install libraries required by the project

Now we can install the required libraries using the following command:

    pip install -r requirements.txt 

This command will install the required Python packages listed in the `requirements.txt` file within the virtual environment.

Now you have a virtual environment set up with the required dependencies installed for the project.

## IDE support

### Using Visual Code as an IDE

Follow the instructions from the [Visual Code Python tutorial](https://code.visualstudio.com/docs/python/python-tutorial) to install the Python plugin for Visual Code. 

### Using PyCharm as an IDE

You can download the latest version of PyCharm from the official website at https://www.jetbrains.com/pycharm/download/. Make sure to select the appropriate installer for your operating system.

Open PyCharm and select **Open** from the welcome screen. Navigate to the directory where you cloned the project and open it.

PyCharm needs to know which version of Python to use for your project. To configure the project interpreter, go to **File > Settings > Project: your-project > Python Interpreter**. Click on the gear icon and select **Add**. From the dropdown menu, select "New Environment" and choose the version of Python in the virtual environment you created.

XXX Double check if PyCharm grabs the virtual environment this way.

You are now good to go to start coding!

---
[Next](lab-4-define-a-new-task-and-test.md)



