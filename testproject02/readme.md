# Django Reactivated Test Project 02

## Install Workflow

Only difference between testproject01 and testproject02 is the method of how the node package of reactivated is installed.


##### 1. Create new environment
Create and activate the new python environment. 

##### 2. Install Python Packages
In the new python environment install the following python packages:
1. `pip install django`
2. `pip install reactivated`

Note:
You may need to install `mypy-extensions` and `simplejson`


##### 3. Create a New Django Project or Navigate to Existing
Create a new django project or navigate to an existing project, in this example we'll be creating a new project called "server" in the current directory. And runserver to verify project works.

Create new project: `django-admin startproject server .`
Run Server: `python manage.py runserver`

##### 4. Setting up Folder Structure
Create three folders (1) client, (2) node_modules, and (3) static. The folder structure should resemble the following:

```
root/
- client/
- node_modules/
- server/
    - __init__.py
    - asgi.py
    - settings.py
    - urls.py
    - wsgi.py
- static/
- manage.py
```


##### 5. Installing Node Package Reactivated
While in the projects root, install the node package reactivated following the documentations method. These packages will install the packages into `node_modules/`
```
npm install reactivated
```

References:
- https://www.reactivated.io/documentation/existing-projects/
- https://classic.yarnpkg.com/en/package/reactivated



##### 6. Setting up Django Server Settings
Following the guidelines (https://www.reactivated.io/documentation/existing-projects/)

In settings.py:

1. Add `reactivated` to the end of the INSTALLED_APPS list. 

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'reactivated'
]
```

2. Add STATICFILES_DIRS `STATICFILES_DIRS = (BASE_DIR / "static/",)`

3. Configuring TEMPLATES to be the following:

```
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
    {
        "BACKEND": "reactivated.backend.JSX",
        "DIRS": [],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.contrib.messages.context_processors.messages",
                "django.template.context_processors.csrf",
                "django.template.context_processors.request",
                "django.template.context_processors.static",
            ]
        },
    },
]
```

##### 7. Setup the Client
Create the following files `.babelrc.json` in root and `index.tsx` inside the client folder.

Folder structure should be:
```
root/
- client/
    - templates/
    - index.tsx
- node_modules/
- server/
- static/
- .babelrc.json
- manage.py
```


**.babelrc.json Setup**
Add the following lines into `.babelrc.json`:
```
{"extends": "reactivated/babel.config.js"}
```

**index.tsx Setup**
```
import React from "react";
import {hydrate} from "react-dom";

import {Provider, getServerData, getTemplate} from "@reactivated";
import {HelmetProvider} from "react-helmet-async";

const {props, context} = getServerData();

const Template = getTemplate(context);

hydrate(
    <HelmetProvider>
        <Provider value={context}>
            <Template {...props} />
        </Provider>
    </HelmetProvider>,
    document.getElementById("root"),
);
```


##### 8. Run Server
Setup is done, run the django server:
```
python manage.py runserver
```




### Bug Message:
```
(djangotest) PS D:\Documents\GitHub\DjangoReactivated\DjangoReactivated\testproject01> python manage.py runserver
[09/Apr/2022 22:43:01,296] Generating interfaces and client side code
[09/Apr/2022 22:43:01,296] Skipping generation as nothing has changed
Traceback (most recent call last):
  File "D:\Documents\GitHub\DjangoReactivated\DjangoReactivated\testproject01\manage.py", line 22, in <module>
    main()
  File "D:\Documents\GitHub\DjangoReactivated\DjangoReactivated\testproject01\manage.py", line 18, in main
    execute_from_command_line(sys.argv)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\__init__.py", line 446, in execute_from_command_line
    utility.execute()
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\__init__.py", line 440, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\base.py", line 414, in run_from_argv
    self.execute(*args, **cmd_options)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\commands\runserver.py", line 74, in execute
    super().execute(*args, **options)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\base.py", line 460, in execute
    output = self.handle(*args, **options)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\commands\runserver.py", line 111, in handle
    self.run(**options)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\core\management\commands\runserver.py", line 118, in run
    autoreload.run_with_reloader(self.inner_run, **options)
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\django\utils\autoreload.py", line 682, in run_with_reloader
    exit_code = restart_with_reloader()
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\reactivated\__init__.py", line 47, in patched_restart_with_reloader
    processes.start_tsc()
  File "C:\Users\Robby\.conda\envs\djangotest\lib\site-packages\reactivated\processes.py", line 10, in start_tsc
    tsc_process = subprocess.Popen(
  File "C:\Users\Robby\.conda\envs\djangotest\lib\subprocess.py", line 951, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "C:\Users\Robby\.conda\envs\djangotest\lib\subprocess.py", line 1420, in _execute_child
    hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
FileNotFoundError: [WinError 2] The system cannot find the file specified
```

