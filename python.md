# Table of Contents
- [Commands](#commands)
- [Virtual Env](#virutalenv)
- [Errors](#errors)

<a name="commands"></a>
## Commands
- `pip list` -- show list of installed packages
- `pip list --outdated --format=freeze` - show list of outdated installed packages
- `pip show <pip-package-name>` -- Show info related a pip package
- `pip install --upgrade <pip-package-name>` - upgrade a package

<a name="virutalenv"></a>
## Virtual Env
- Create python venv
```
$ virtualenv -p /usr/bin/python2.7 my_project
$ source my_project/bin/activate
$ pip freeze > requirements.txt
$ dactivate
```

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
