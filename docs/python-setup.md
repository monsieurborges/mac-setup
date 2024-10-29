# Python

![Python](../assets/python.png?raw=true)

[Python](https://www.python.org) is a programming language that lets you work quickly
and integrate systems more effectively.

## Install Pyenv and Python 3.10.7

[Pyenv](https://github.com/pyenv/pyenv) lets you easily switch between multiple versions of Python.

* **Option 1** Easy install using mac-setup:

    ```bash
    bash <(curl -fsSL raw.githubusercontent.com/monsieurborges/mac-setup/master/install) python310
    ```

* **Option 2** Install on your own:

  1. Install pyenv:

      ```bash
      brew install pyenv
      ```

  2. Install pyenv-virtualenvwrapper :

      ```bash
      brew install pyenv-virtualenvwrapper
      ```

  3. Init Pyenv:

      ```bash
      eval "$(pyenv init -)"
      ```

  4. List the available Python versions:

      ```bash
      pyenv install --list
      ```

  5. Install the Python versions you want using the example below:

      ```bash
      # Python 3.10.7
      PYTHON_CONFIGURE_OPTS="--enable-framework" pyenv install 3.10.7
      ```

## Set the Python version you want as default

1. Set the default Python version:

    ```bash
    pyenv global 3.10.7
    ```

2. Set up the virtualenvwrapper:

    ```bash
    pyenv virtualenvwrapper
    ```
