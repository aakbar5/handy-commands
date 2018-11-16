# Table of Contents
- [Commands](#commands)
- [Virtual Env](#virutalenv)
- [Pip upgrade](#pip_upgrade)
- [Errors](#errors)

<a name="commands"></a>
## Commands

- `pip list` -- show list of installed packages
- `pip list --outdated --format=freeze` - show list of outdated installed packages
- `pip show <pip-package-name>` -- Show info related a pip package
- `pip install --upgrade <pip-package-name>` - upgrade a package
- `pip install --download /tmp/pip_wheels -r requirements.txt` - Download specified wheels in a directory
- `pip install --no-cache-dir --find-links=/tmp/pip_wheels -r requirements.txt` - Install wheels from the locally directory before fetching it from PyPI repo + No cache the wheel.

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
