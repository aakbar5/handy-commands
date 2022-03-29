# Table of Contents
- [Pip](#pip)
- [Python](#python)
- [Python-config](#python_config)
- [Virtual Env](#virutalenv)
- [Pip upgrade](#pip_upgrade)
- [Samples](#samples)
- [Errors](#errors)

<a name="pip"></a>
## Pip

- `pip list` - Show list of installed packages
- `pip list --outdated --format=freeze` - Show list of outdated installed packages
- `pip install --upgrade <pip-package-name>` - Upgrade a package
- `pip install "package>=0.2,<0.3"` - Install a specific version of pip package
- `pip show <pip-package-name>` - Show info related a pip package
- `pip freeze > reqs.txt` - Generate reqs.txt having all installed packages and their versions
- `pip install -r reqs.txt` - Install package(s) specified in the reqs.txt
- `pip download -d /tmp/pip_wheels -r reqs.txt` - Download specified wheels in a directory
- `pip install --no-cache-dir --find-links=/tmp/pip_wheels -r reqs.txt` - Install wheels from the locally directory before fetching it from PyPI repo + No cache the wheel.

<a name="python"></a>
## Python

- `python -m site` - System wide python installation folder
- `python -m site --user-site` - Use specific python isntallation folder
- `python -c "import site; print(site.getsitepackages())"` - Get site package folders in python

<a name="python_config"></a>
## Python-config

- `python-config --prefix` - Python installation folder
- `python-config --exec-prefix` - Python installation folder
- `python-config --includes` - Show python includes folders with -I
- `python-config --libs` - Show python libraries
- `python-config --cflags` - Show CFLAGS used by Python
- `python-config --ldflags` - Show LDFLAGS used by Python
- `python-config --extension-suffix` - Show extension of python libs
- `python-config --configdir` - Show config directory of python

<a name="virutalenv"></a>
## Virtual Env

- Create python venv
```
$ virtualenv -p /usr/bin/python2.7 my_project
$ source my_project/bin/activate
$ source my_project/bin/activate.fish (Using fish shell)
$ pip freeze > reqs.txt
$ dactivate
```

<a name="pip_upgrade"></a>
## Pip upgrade

- `pip install --upgrade pip` - To upgrade pip; not recommended, I have always found pip broken afterwards
- `python -m pip install --upgrade pip` - Recommended command to upgrade pip.

<a name="sample"></a>
## Samples

- [Single line for loop](https://github.com/aakbar5/handy-python/blob/master/general/single_line_loop.py)
- [Compare two lists](https://github.com/aakbar5/handy-python/blob/master/general/list_cmp.py)
- [Get list of diff](https://github.com/aakbar5/handy-python/blob/master/general/list_diff.py)
- [Convert list of lists to list](https://github.com/aakbar5/handy-python/blob/master/general/list_of_lists_to_list.py)
- [Convert dictionary to tuple](https://github.com/aakbar5/handy-python/blob/master/general/dict_to_tuple.py)
- [Convert dictionary to tuple by order](https://github.com/aakbar5/handy-python/blob/master/general/dict_to_tuple_by_order.py)
- [Convert list of tuple to list](https://github.com/aakbar5/handy-python/blob/master/general/tuple_list_to_list.py)
- [Convert tuple to list](https://github.com/aakbar5/handy-python/blob/master/general/tuple_to_list.py)
- [Path manipulation](https://github.com/aakbar5/handy-python/blob/master/general/path.py)
- [Scan a folder](https://github.com/aakbar5/handy-python/blob/master/general/folder_scan.py)
- [Watch a folder](https://github.com/aakbar5/handy-python/blob/master/general/folder_watch.py)
- [Singleton class object](https://github.com/aakbar5/handy-python/blob/master/general/singleton.py)
- [Thread](https://github.com/aakbar5/handy-python/blob/master/general/py_thread.py)
- [Read/Write JSON data](https://github.com/aakbar5/handy-python/blob/master/json/json_test.py)
- [Read/Write YAML data](https://github.com/aakbar5/handy-python/blob/master/yaml/yaml_test.py)
- [PyQt5 -- Hello world](https://github.com/aakbar5/handy-python/blob/master/qt5/qt_helloworld.py)
- [PyQt5 -- Dialog from UI file](https://github.com/aakbar5/handy-python/blob/master/qt5/qt_dialog_ui.py)
- [PyQt5 -- QThread](https://github.com/aakbar5/handy-python/blob/master/qt5/qt_qthread.py)

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
