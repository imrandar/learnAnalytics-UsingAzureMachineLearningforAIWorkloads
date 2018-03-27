# Behind the scenes: Docker images and Conda environments

## What is Conda?

Conda is a multi-platform, open-source package management system. Fundamentally it tries to solve replicability by creating a clean environment in which our code and all its dependencies can be placed and run. It is primarily used for Python, but can be used for multiple languages (including R). It is generally packaged and distributed through the Anaconda python distribution supported by Anaconda, Inc (a Microsoft partner).

In the context of Python, Conda is similar to `pip` and can be used to install and update Python packages, but Conda goes beyond that and can also install library dependencies outside of Python, such as C++ dependencies.

With AML projects, we use Docker for managing system requirements and dependencies between compute environments (local, remote, Spark). Within Docker itself, we use Conda to manage Python dependencies for a given project. We use the command line `conda` command to interact with `conda` (in the case of Python, we also have some control in Jupyter Notebooks)

CRITICAL NOTE: By default, conda is not used for local script runs -- local runs by default will use the `root` python environment instead. This can problems if we work on multiple projects, as different projects may have different types of dependencies. As a general rule, it is better to use Docker even when working locally.

Open Workbench and create a new project called `hello_bootcamp`, make it a blank project and place it in the `Documents` folder. Open the project and go to **File > Open Command Prompt** to access the command line from within the project parent folder. Type `python`, then in the python console paste this in:

```
import sys
print(sys.executable)

import matplotlib
print("matplotlib version:", matplotlib.__version__)
```

Note the path to the Python executable and the `matplotlib` version. Now type `exit()` to leave the Python console and return to the command prompt.  

If working locally without the use of Conda environments, we would be using our local root Python installation, which means that all projects would rely on the same environment and where conflicting dependencies can cause collusion, and where missing system dependencies can cause headaches when going from development to staging or production. Therefore, if working locally it is still better to leverage Docker (and Conda within Docker) instead of relying on the root Python executable as we did above. Let's now see how we can do this.

To see if Conda is installed, type `conda --version`. Now type `conda list` to see a listing of installed python packages. Some of these packages are installed using `pip` or `easy_install`, some are installed using `conda install`.

Open the project with Code by going to **File > Open Project (Code)**. Open the `conda_dependencies.yml` file and examine the content. What package dependencies are specified here? Add `matplotlib=2.0.2` to the list of dependencies for this project then save the file. If your version of `conda` is less than `4.4.0`, you will need to comment out the following three lines (in Code, use **CTRL+/** to comment and uncomment a code chunk):

```
- --index-url https://azuremldownloads.azureedge.net/python-repository/preview
- --extra-index-url https://pypi.python.org/simple
- azureml-requirements
```

Save your changes and return to the command prompt to create the Conda environment by typing this:

```
conda env create -f aml_config/conda_dependencies.yml
```

This creates an environment called `project_environment` by default (we can rename it using the `-n` switch, or by changing the `name` field in the `conda_dependencies.yml` file). Once an environment is created, it needs to be activated by running `activate project_environment`. Activate the environment and run `python`. Then in the python console type 

```
import sys
print(sys.executable)

import matplotlib
print("matplotlib version:", matplotlib.__version__)
```

Compare the path to the Python executable and the `matplotlib` version to the one we obtained earlier. Type `exit()` to return from the Python console to the command line. We can type `conda env list` to see available Conda environments. A star indicates which one is active.

To deactivate an environment simply type `deactivate` (or `source deactivate` if using `bash`). Note that an environment is only active for the open Command Prompt session and will be deactivated if we close the session. We can also permanently remove this environment using `conda env remove -n project_environment`. Deactivate and remove the Conda environment.

So far we manually created and activated Conda environments. This is useful in order to see some of what happens behind the scene when we run an experiment in AML. However, in practice the above steps are built-in and automatically handled by AML itself.

Return to Code and create a new script and call it `my_script.py` then paste in the following into it:

```
import sys
print(sys.executable)

import matplotlib
print("matplotlib version:", matplotlib.__version__)
```

Now save changes by going to **File > Save All**.

Return to Workbench and verify that the changes are visible. To run `my_script.py` using Docker as the compute environment, we must first prepare the Docker environment. This will take a few minutes, after which, we can sumbit the experiment. Return to the command prompt run the following command: 

```
az ml experiment prepare -c docker
```

The above command prepares a new Docker image that we can run our experiment in. We can directly access this image by typing `docker image list` and copying the name of the image (the one prefixed with `azureml_`). Then we type `docker run -it <image_name>` which puts us directly inside the command line of the Docker image. In here we can type `python` to launch a Python session and paste in the same Python code from above to check the Python executable path and the `matplotlib` version. We can then type `exit()` to leave the Python console and type `exit` to leave the Docker shell and return to the main command prompt. Manually logging into the Docker environment is tedious and not necessary in most cases. Instead we can run the following command to directly run the script in the Conda environment inside the Docker image.

```
az ml experiment submit -c docker my_script.py
```

In Workbench, go to the **Jobs** panel on the right and click on the green **Completed** button to see any results printed by the script. Does the `matplotlib` version number match the version number in `conda_dependencies.yml`?
Conda creates an execution environment for our project and binds our Python scripts and their dependencies so that its execution environment can be isolated from that of other projects. By submitting the above command, we created a Docker image with our project and used Conda to handle the script and its dependencies, which Conda does by creating a new "environment" for the project. We created a Conda environment automatically in the last step is because in the `aml_config/docker.runconfig` file we point to `conda_dependencies.yml` and we set `PrepareEnvironment: true`.

Most of us do not develop or test our Python scripts from the Python console. Instead we prefer to use an IDE like Code or Jupyter Notebooks. To launch a Jupyter Notebook session for a given project, return to the command prompt and run `az ml notebook start`. This will open up a browser session (`localhost:8888`) and present our project parent directory. On the right side, click on **New** and examine the content of the dropdown. It should include `hello_bootcamp local` and `hello_bootcamp docker` with are the Conda environments associated with our project. Click on `hello_bootcamp docker` and paste in and run the following code in the cell of the new Jupyter Notebook that opens (To run a cell in a Jupyter Notebook, simply select the cell and press **CTRL+Enter**).

```
import sys
print(sys.executable)

import matplotlib
print("matplotlib version:", matplotlib.__version__)
```

Check the `matplotlib` version and the Python executable path and confirm that it matches what we earlier. On the Jupyter Notebook, go to **File > Close and Halt** to close the Notebook, then close the Jupyter tab on the browser. Finally, return to the command prompt and press **CTRL+C** to stop the Notebook server and return to the prompt.

As we saw above, leveraging Docker and Conda gives us the ideal environment to run our projects, and we can leverage them both from the command line and Jupyter. However, Docker is not always available on Windows machines. On the DSVM for example, we need to use `DX_v3` instances (and enable Hyper-V) in order be able to install and use Docker. When docker is not an option and we can only work locally, then we can (and definitely should) still use Conda. Let's see how this can be done.

From the command prompt, create a new Conda environment by pointing to `conda_dependencies.yml` and use `-n hello_env` to rename it from `project_environment` to `hello_env` (renaming the Conda environment is an optional step). Go to the `aml_config/local.compute` file and point the Python location from the root Python directory to the Python directory specific to this environment. We can do so by replacing `pythonLocation: "python"` with `pythonLocation: "%USERPROFILE%/AppData/Local/amlworkbench/Python/envs/hello_env/python.exe"`. To make sure that this works, run `my_script.py` as a local experiment and inspect the results generated by the script.

It is likely that the run in the last step fails because some AML dependencies are not properly set up in the `hello_env` environment. Create a new file called `localenv_conda_dependencies.yml` and place it in `aml_config`. Place the following content inside of it and save it:

```
# Conda environment specification. The dependencies defined in this file will be
# automatically provisioned for runs against docker, VM, and HDI cluster targets.

# Details about the Conda environment file format:
# https://conda.io/docs/using/envs.html#create-environment-file-by-hand

# For Spark packages and configuration, see spark_dependencies.yml.

name: hello_env
dependencies:
  - python=3.5.2
  - ipykernel=4.6.1
  - matplotlib=2.0.2
  ## needed by AML:
  - psutil=5.2.2
  ## needed by azureml.logging below. If left off here, pip tries to build it from source. Common compiler issues make it easier to install with conda rather than pip (on windows):
  - scipy=0.18.1

  - pip:
    # The API for Azure Machine Learning Model Management Service.
    - azure-ml-api-sdk==0.1.0a10

    # aml specific:
    - https://azuremldownloads.blob.core.windows.net/wheels/latest/azureml.assets-1.0.0-py3-none-any.whl?sv=2016-05-31&si=ro-2017&sr=c&sig=xnUdTm0B%2F%2FfknhTaRInBXyu2QTTt8wA3OsXwGVgU%2BJk%3D
    - https://azuremldownloads.blob.core.windows.net/wheels/latest/azureml.logging-1.0.79-py3-none-any.whl?sv=2016-05-31&si=ro-2017&sr=c&sig=xnUdTm0B%2F%2FfknhTaRInBXyu2QTTt8wA3OsXwGVgU%2BJk%3D
    # note the version number of AML is in the whl name:
    - https://azuremldownloads.blob.core.windows.net/wheels/latest/azureml.dataprep-0.1.1711.15323-py3-none-any.whl?sv=2016-05-31&si=ro-2017&sr=c&sig=xnUdTm0B%2F%2FfknhTaRInBXyu2QTTt8wA3OsXwGVgU%2BJk%3D
    - applicationinsights==0.11.0
```

Notice in the above file we refer directly to the Conda environment using `name: hello_env`. In `aml_config/local.runconfig` change the `CondaDependenciesFile` to point to `"aml_config/localenv_conda_dependencies.yml"` instead. As noted above, AML does not use this field for local runs. In spite of that, it's useful to make sure that the documents are consistent within a project. Additionally, it's useful to note the differences between `conda_dependencies.yml` and `localenv_conda_dependencies.yml`. The delta between those files correspond to dependencies that AML takes care of automatically when generating a docker image for experiment execution. 

Return to the command prompt and remove the Conda environment `hello_env` using the following command in order to start from a clean slate:

```
conda env remove -n hello_env
```
Now create a new Conda environment called `hello_env` by using the  `aml_config/localenv_conda_dependencies.yml` file we just created (Note that because the new dependencies file has the `name` field appropriate defined, we do not need to use the `-n` argument).

From the command line, use `az ml` to run `my_script.py` again. This time we successfully run `my_script.py` in a local Conda environment. Does the `matplotlib` package version match what we specified in `localenv_conda_dependencies.yml`?  

It bears repeating that using Conda locally is the right approach if Docker is not available, but otherwise it's better to use Conda inside a Docker image because Docker can also handle certain system dependencies that Conda wasn't built to handle.

## Conclusions

When developing our code on a local machines (laptops) or on DSVMs:

1. Try to always use versions for packages in `conda_dependencies.yml`.
2. Leverage Docker images and Conda environments for every project. A Docker image manages system dependencies as well.
3. If Docker is not available, still leverage Conda environments but be prepared to deal with additional system dependencies when moving the project to staging or production. The most common problems are compiler-related (such as C++ and Fortran).
4. If leveraging AML + conda without docker, be prepared to manage additional packages beyond those normally needed for docker execution with AML. See above for a list of some of the additional packages that could be required.
4. If we don't use Docker images and don't rely on Conda environments (in other words if we always run projects using the local Python root executable), then we should expect to run into headaches when 
  - upgrading any major Python packages (especially the data science and machine learning packages),
  - moving between different projects with different package dependencies
  - deploying the project to staging and production.
  - we have packages installed in the user directory from sources such as `pip install --user`
  - upgrade versions of AML, as some of the dependencies are tied to the AML version number.
