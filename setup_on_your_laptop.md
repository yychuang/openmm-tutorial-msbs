# Installation of OpenMM on your laptop

To run OpenMM simulations on your laptop, we will work with a standardized Python environment, Anaconda, which is supported on all major operating systems.

Some of the more specialized packages (required for tutorial 07) are only used on Unix systems (Linux and macOS) by researchers in the field, and these have poor support on Windows. Luckily, on Windows 10, Microsoft distributes the Windows Subsystem for Linux 2 (WSL2), as of a May 2019. This allows you to run Linux inside your Windows operating system, giving you access to a small-scale version the software environment used on high-performance cluster. More details can be found in the [Official WSL installation instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

Take the following steps:

1. Download the Anaconda Python 3.7 installer for your operating system. The 64-bit version is strongly preferred if your hardware and operating system support it. (Most modern laptops do.) The installer can be downloaded from [the anaconda website](https://www.anaconda.com/distribution/).

2. Run the Anaconda Python installer.

    a. Windows: open the `.exe` file.

    b. macOS: ???

    c. Linux: run `./Anaconda3-*-Linux-x86_64.sh -b -p ${HOME}/anaconda`.
       Add the following line to your `~.bashrc` file, which makes it convenient to
       activate the conda installation:
       `alias c='source ~/anaconda/bin/activate'`.

3. Start a command-line prompt.

    a. Windows: run the application "Anaconda prompt" from the start menu.

    b. macOS: ???

    c. Linux: open your preferred terminal emulator and enter the command alias `c`.


4. Configure conda and install OpenMM and other useful tools).

   a. Windows: copy the following line by line in the anaconda terminal.
      (The package `openmmforcields` is not supported and some special tricks
      are needed to install the latest version of the `openforcefields` package.
      Both are needed in toturial 07.)

      ```bash
      conda config --add channels conda-forge
      # The following creates a conda environment called openmm
      # in which a several packages are installed.
      conda create -n openmm "python<3.8" git spyder jupyter numpy pandas scipy matplotlib ipympl rdkit openbabel openmm mdtraj nglview pymbar pdbfixer parmed
      # Activate the environment just created.
      conda activate openmm
      # Install openmm and a few more related tools, avoiding the openforcefield and openmmforcefields packages.
      conda install -c omnia openforcefield openmoltools  # openmmforcefields
      # Install the latest version of the openforcefield package directly from github.
      pip install git+https://github.com/openforcefield/openforcefields@1.3.0
      # Enable nglview in jupyter notebooks
      jupyter-nbextension enable nglview --py --sys-prefix
      ```

   b. macOS and Linux: run the following commands.
      You can copy-paste all lines in one go.

      ```bash
      conda config --add channels conda-forge
      # The following creates a conda environment called openmm
      # in which a several packages are installed.
      conda create -n openmm "python<3.8" spyder jupyter numpy pandas scipy matplotlib ipympl rdkit openbabel openmm mdtraj nglview pymbar pdbfixer parmed
      # Activate the environment just created.
      conda activate openmm
      # Install openmm and a few more related tools.
      conda install -c omnia openforcefield openforcefields openmoltools openmmforcefields
      # Enable nglview in jupyter notebooks
      jupyter-nbextension enable nglview --py --sys-prefix
      ```

5. Test your OpenMM installation by entering the following command on the command prompt:

    ```bash
    python -m simtk.testInstallation
    ```

    You should see the following output (or something similar):

    ```
    OpenMM Version: 7.4
    Git Revision: b71e92d9ccef7c98cfd6b862eb34095d94fa1b05

    There are 3 Platforms available:

    1 Reference - Successfully computed forces
    2 CPU - Successfully computed forces
    3 OpenCL - Error computing forces with OpenCL platform

    OpenCL platform error: Error initializing context: clGetDeviceIDs (-1)

    Median difference in forces between platforms:

    Reference vs. CPU: 6.31232e-06

    All differences are within tolerance.
    ```


6. Now is a good time to go through the [Jupyter Notebook Introduction](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html#starting-the-notebook-server) to become familiar with its main features.

    Start a Jupyeter notebook, e.g. by entering `jupyter notebook` in the command-prompt. Create a new Python 3 notebook, enter the following two lines in the first code cell and execute it by clicking on the play button (or typing Shift+Enter):

    ```python
    import simtk.testInstallation
    simtk.testInstallation.main()
    ```

    If you get a "kernel error" on Windows, you might be running into the following issue (or a similar one): https://github.com/jupyter/notebook/issues/4907. this should be in principle solved with the `jupyter_client>=5.3.4` and `jupyter_core>=4.6.1`, available on conda-forge as of November 17, 2019. In case you still run into this issue, try to downgrade `jupyter_client` to version 5.3.1 as follows on your Conda prompt:

    ```
    conda install jupyter_client=5.3.1
    ```


7. Start Spyder, a simple Python integrated development environment, enter the same two lines from the previous point in the editor and execute (click the green Play button). You should see the same output. Unlike notebooks, source code in Spyder is not grouped into cells. Unlike notebooks, a script in Spyder does not know remember variables from previous executions.


8. Install VMD, which will be used for showing some visualization good practices.

   a. Any operating system: go to [the VMD download page](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD) and follow instructions.

   b. On Linux, the following also works but might not be fully legal:

      ```
      # We'll also install VMD, in a separate conda environment
      # because it seems to interfere with jupyter notebooks.
      conda create -n vmd vmd
      # To start VMD, switch conda to the conda environment and start
      # VMD.
      conda activate vmd
      vmd
      ```
