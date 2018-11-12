{% comment %}
# Django Project Package Template

This is a template for a [Django](https://www.djangoproject.com/) 2.x project.

The project has a layout so that it can be build as a [wheel](https://github.com/pypa/wheel), one type of [Python packages](https://pypi.org/help/#packages).

## Features

*   `setup()` is configured using a [`setup.cfg` file](https://setuptools.readthedocs.io/en/latest/setuptools.html#configuring-setup-using-setup-cfg-files).
*   The version is defined in `{{ project_name }}.__version__`.
*   Source code is located in a `src` directory to avoid side effects.
*   All apps use the `{{ project_name }}.apps` namespace.
*   All configuration modules use the `{{ project_name }}.conf` namespace.
*   `{{ project_name }}.conf.settings` uses [`pathlib`](https://docs.python.org/3.7/library/pathlib.html) instead of `os` and `os.path`.
*   All code follows the [Black](https://github.com/ambv/black) code style.
*   All docstrings follow [PEP 257](https://www.python.org/dev/peps/pep-0257/) conventions.
*   [Django Debug Toolbar](https://github.com/jazzband/django-debug-toolbar), [IPython](https://ipython.org/) and [check-manifest](https://github.com/mgedmin/check-manifest) are already added to the development dependencies.

## Usage

Use the following [startproject](https://docs.djangoproject.com/en/stable/ref/django-admin/#django-admin-startproject) command to create a new project using this template:

```console
python -m django startproject --extension=cfg,gitignore,gitkeep,in,md,sublime-project \
    --template=https://github.com/keimlink/django-project-package-template/archive/master.zip \
    name [directory]
```

_Tip: If you want to create the project in your current working directory use `.` as directory argument._

All text below the horizontal line is the template for the new project's README.

---
{% endcomment %}# {{ project_name|title }}

Describe your project in one sentence.

## Quickstart

Install the project and the development dependencies into a [virtual environment](https://docs.python.org/3.7/tutorial/venv.html):

```console
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
python -m pip install --editable .[dev]
./manage.py migrate
./manage.py runserver
```

## Starting a New App

First create a new directory in the `apps` directory:

```console
mkdir src/{{ project_name }}/apps/name
```

Then pass the path to the new directory to the [startapp](https://docs.djangoproject.com/en/{{ docs_version }}/ref/django-admin/#django-admin-startapp) command:

```console
./manage.py startapp name src/{{ project_name }}/apps/name
```

## Deployment

1.  Add your favorite WSGI HTTP server, e.g.  [Gunicorn](https://gunicorn.org/), to `install_requires` in `setup.cfg`.
2.  [Check](https://github.com/mgedmin/check-manifest) if all files are included in the package:
    ```console
    check-manifest
    ```
1.  [Build](https://packaging.python.org/tutorials/packaging-projects/#generating-distribution-archives) a [wheel](https://github.com/pypa/wheel) of the project.
    ```console
    ./setup.py bdist_wheel
    ```
1.  Copy the wheel file from the `dist` directory to the server to be deployed.
2.  Install the wheel and [collect the static files](https://docs.djangoproject.com/en/{{ docs_version }}/ref/contrib/staticfiles/#django-admin-collectstatic):
    ```console
    python -m pip install --find-links=/path/to/wheel_dir {{ project_name }}
    export DJANGO_SETTINGS_MODULE={{ project_name }}.conf.settings
    django-project collectstatic --no-input
    ```
6.  Start Gunicorn like this:
    ```console
    gunicorn {{ project_name }}.conf.wsgi
    ```
