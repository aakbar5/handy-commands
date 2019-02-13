# Table of Contents
- [Commands](#commands)
- [Virtual Env](#virutalenv)
- [Pip upgrade](#pip_upgrade)
- [Errors](#errors)

<a name="commands"></a>
## Commands

- `pip list` -- Show list of installed packages
- `pip list --outdated --format=freeze` -- Show list of outdated installed packages
- `pip show <pip-package-name>` -- Show info related a pip package
- `pip install --upgrade <pip-package-name>` -- Upgrade a package
- `pip install "package>=0.2,<0.3"` -- Install a specific version of pip package
- `pip download -d /tmp/pip_wheels -r requirements.txt` -- Download specified wheels in a directory
- `pip install --no-cache-dir --find-links=/tmp/pip_wheels -r requirements.txt` -- Install wheels from the locally directory before fetching it from PyPI repo + No cache the wheel.
- `python-config --prefix` -- Python installation folder
- `python-config --exec-prefix` -- Python installation folder
- `python-config --includes` -- Show python includes folders with -I
- `python-config --libs` -- Show python libraries
- `python-config --cflags` -- Show CFLAGS used by Python
- `python-config --ldflags` -- Show LDFLAGS used by Python
- `python-config --extension-suffix` -- Show extension of python libs
- `python-config --configdir` -- Show config directory of python
- `python -m site` -- System wide python installation folder
- `python -m site --user-site` -- Use specific python isntallation folder
- `python -c "import site; print(site.getsitepackages())"` -- Get site package folders in python

<a name="virutalenv"></a>
## Virtual Env

- Create python venv
```
$ virtualenv -p /usr/bin/python2.7 my_project
$ source my_project/bin/activate
$ source my_project/bin/activate.fish (Using fish shell)
$ pip freeze > requirements.txt
$ dactivate
```
<a name="pip_upgrade"></a>
## Pip upgrade

- `pip install --upgrade pip` -- To upgrade pip; not recommended, I have always found pip broken afterwards
- `python -m pip install --upgrade pip` -- Recommended command to upgrade pip.

<a name="errors"></a>
## Errors

- Different errors faced during development
### 1
- Error
```
The directory '~/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '~/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
```
- Solution
`sudo -H pip3 install <package-name>`

### 2
- Error
```
$ pip3 install numpy
Traceback (most recent call last):
  File "/usr/bin/pip3", line 9, in <module>
    from pip import main
ImportError: cannot import name 'main'
```
Solution
`sudo python3 -m pip uninstall pip && sudo apt install python3-pip --reinstall`
