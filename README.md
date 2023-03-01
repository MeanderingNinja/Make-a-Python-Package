# Make-a-Python-Package
This repo demonstrates my knowledge on python packages and building one. 

### [What is a package in Python?](https://www.freecodecamp.org/news/how-to-create-and-upload-your-first-python-package-to-pypi/)

A Python package is a directory that contains a bunch of modules with a dependency file called `__init__.py`. The purpose of the `__init__.py` file is to define what should happen when the package is imported.

If you don't include an `__init__.py` file in a directory, it will still be considered a package, but it will not have any special behavior. This means that you won't be able to import any variables, functions, or classes defined within the package, and the package will not be initialized in any special way.

It is considered a best practice to include an `__init__.py` file in a package, even if it is empty, to signal to other developers that the directory is intended to be a package.

### [Package structure](https://www.freecodecamp.org/news/how-to-create-and-upload-your-first-python-package-to-pypi/)

Packages don't only contain modules, though. They consist of top-level scripts, documentation, and tests, as well. The following example shows how a basic Python package can be structured:

```
package_name/
    docs/
    scripts/
    src/
        package_a
            __init__.py
            module_a.py
        package_b
            __init__.py
            module_b.py
    tests/
    	__init__.py
        test_module_a.py
        test_module_b.py
    LICENSE.txt
    CHANGES.txt
    MANIFEST.in
    README.txt
    pyproject.toml
    setup.py
    setup.cfg
```

Let's understand what each file in the tree above is used for:

- **package_name**: represents the main package.
- **docs**: includes documentation files on how to use the package.
- **scripts/**: your top-level scripts.
- **src**: where your code goes. It contains packages, modules, sub-packages, and so on.
- **tests**: where you can put unit tests.
- **LICENSE.txt**: contains the text of the license (for example, MIT).
- **CHANGES.txt**: reports the changes of each release.
- **MANIFEST.in**: where you put instructions on what extra files you want to include (non-code files).
- **README.txt**: contains the package description (markdown format).
- **pyproject.toml**: to register your build tools.
- **setup.py**: contains the build script for your build tools.
- **setup.cfg**: the configuration file of your build tools.

Note: Choose either `setup.py` or `setup.cfg`. They contains information like the version, packages, and files to include, along with any requirements, but are written in different formats.

# A sample Python package I created

```bash
# This package is made to watch new files in a specified directory. 
# When a new file is found, it loads the data in it to a `Postgresql` database.
# File Structure
.
└── cat_data/
    ├── CatDataSchema/
    │   ├── __init__.py
    │   ├── alembic/
    │   │   ├── env.py
    │   │   ├── README
    │   │   ├── script.py.mako
    │   │   └── versions/
    │   │       └── 9722539bc713_initiate_model_20220823.py
    │   ├── alembic.ini
    │   ├── cli.py
    │   ├── config.py
    │   ├── models.py
    │   └── simple_etl.py
    ├── MANIFEST.in
    ├── README.md
    ├── requirements.txt
    ├── setup.py
    ├── VERSION
    └── .gitignore
```

This package provides two command-line utilities- `cat_data_watcher` and `cat_data_migrate`. This is made possible by using the `console_scripts` key in the `entry_points` section of the `setup.py` file, as shown below:

```python
from setuptools import setup, find_namespace_packages

setup(
...
    entry_points={
        "console_scripts": [
            "cat_data_watcher=CatDataSchema.cli:cat_data_watcher",
            "cat_data_migrate=CatDataSchema.cli:migrate",
        ]
    },
...
    )
```

When the package is installed using pip, its entry points are **automatically registered with the system** and can be invoked from the command line or from other Python scripts.

# Installing the package

**During development:**

Navigate to the directory where the package's `setup.py` file is located, then use `pip install -e .` command to install a package in editable mode, which means that the package is installed as a symlink or shortcut to the project directory rather than being copied to the Python environment.

**For production:**

Use `pip install .` command to install packages in non-editable mode, which means that the package is installed in the Python environment as a separate copy. This ensures that the package is self-contained and does not depend on the original project directory.

### Uninstalling the package:

Use `pip uninstall <package_name>`

```bash
cat_dev@cattechserver:~/cat_tech/cat_data_pipeline_venv/cat_data$ pip uninstall catDataSchema
Found existing installation: CatDataSchema 0.1.0
Uninstalling CatDataSchema-0.1.0:
  Would remove:
    /home/cat_dev/.local/bin/cat_data_migrate
    /home/cat_dev/.local/bin/cat_data_watcher
    /home/cat_dev/.local/lib/python3.10/site-packages/CatDataSchema.egg-link
Proceed (Y/n)? y
  Successfully uninstalled CatDataSchema-0.1.0
```
