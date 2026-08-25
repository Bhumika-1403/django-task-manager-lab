# django-task-manager-lab

django-task-manager-lab is a hands-on lab and simple Task Manager web application built with Django and Python. This repository guides you through creating a Django project, defining a Task model, connecting to a database, and building a basic UI to manage tasks.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
  - [Clone the repo](#clone-the-repo)
  - [Create a virtual environment](#create-a-virtual-environment)
  - [Install dependencies](#install-dependencies)
  - [Database setup & migrations](#database-setup--migrations)
  - [Create a superuser (optional)](#create-a-superuser-optional)
  - [Run the development server](#run-the-development-server)
- [Project Structure](#project-structure)
- [Task model (example)](#task-model-example)
- [Routes / Views](#routes--views)
- [Testing](#testing)
- [Deployment notes](#deployment-notes)
- [Contributing](#contributing)
- [License](#license)
- [Maintainer / Contact](#maintainer--contact)

---

## Project Overview

This lab walks through the core Django development flow:

Model → Migrations → Views → URLs → Templates → Forms → Database

By the end of the lab you will have a working Task Manager that supports viewing tasks and adding new tasks via the app as well as managing tasks through the Django admin.

---

## Features

- Create, read, update, and delete tasks (CRUD)
- Task status (Pending, In Progress, Completed)
- Created timestamp for each task
- Simple UI using Django templates (easy to extend)

Optional enhancements you can add:

- Due dates, priority, tags, and filtering
- User authentication and per-user task lists
- REST API with Django REST Framework

---

## Prerequisites

- Python 3.8+
- pip
- Optional: virtualenv or venv
- Basic familiarity with Python and the command line

---

## Quick Start

Follow these steps to run the project locally.

### Clone the repo

```bash
git clone https://github.com/Bhumika-1403/Brainery.git
cd Brainery
```

### Create a virtual environment

Using venv (recommended):

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows (PowerShell)
.\.venv\Scripts\Activate.ps1
```

### Install dependencies

If the repository includes a requirements file:

```bash
pip install -r requirements.txt
```

If there is no requirements file yet, install Django:

```bash
pip install django
```

### Database setup & migrations

This lab uses SQLite by default for development. Apply migrations:

```bash
python manage.py migrate
```

If you change models, create and apply migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

### Create a superuser (optional)

To access Django Admin:

```bash
python manage.py createsuperuser
```

### Run the development server

```bash
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser. A common task listing page is available at `/tasks/` if the app routes are configured as described in this lab.

---

## Project Structure

A typical layout for this project looks like:

```
Brainery/
├─ manage.py
├─ requirements.txt
├─ README.md
├─ brainery/        # Django project settings
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
└─ tasks/           # Django app
   ├─ migrations/
   ├─ models.py
   ├─ views.py
   ├─ urls.py
   └─ templates/tasks/
```

Adjust the structure above to match the repository if it differs.

---

## Task model (example)

Use this example Task model or adapt your existing model in `tasks/models.py`:

```python
from django.db import models

class Task(models.Model):
    STATUS_PENDING = 'pending'
    STATUS_IN_PROGRESS = 'in_progress'
    STATUS_COMPLETED = 'completed'

    STATUS_CHOICES = [
        (STATUS_PENDING, 'Pending'),
        (STATUS_IN_PROGRESS, 'In Progress'),
        (STATUS_COMPLETED, 'Completed'),
    ]

    title = models.CharField(max_length=255)
    description = models.TextField(blank=True)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default=STATUS_PENDING)
    created_at = models.DateTimeField(auto_now_add=True)
    due_date = models.DateTimeField(null=True, blank=True)
    priority = models.IntegerField(default=0)

    def __str__(self):
        return self.title
```

Remember to run makemigrations and migrate after editing models.

---

## Routes / Views

A minimal set of routes you can provide in `tasks/urls.py`:

- `/tasks/` — list all tasks
- `/tasks/new/` — form to create a new task
- `/tasks/<id>/edit/` — edit a task

In `tasks/views.py`, create views that query the Task model, render templates, handle form validation, and redirect after successful submissions.

Example: display tasks at `/tasks/` using a template `templates/tasks/list.html`.

---

## Testing

If you add tests, run them with:

```bash
python manage.py test
```

Write unit tests for models, views, and forms to ensure behavior remains correct as you extend the app.

---

## Deployment notes

When deploying to production:

- Use a production-ready database such as PostgreSQL or MySQL
- Set `DEBUG = False` and configure `ALLOWED_HOSTS`
- Store sensitive settings (SECRET_KEY, DB credentials) in environment variables
- Serve static files with WhiteNoise, a web server (Nginx), or a CDN
- Use Gunicorn (or an ASGI server like Daphne) behind a reverse proxy

See Django's deployment checklist for details: https://docs.djangoproject.com/en/stable/howto/deployment/checklist/

---

## Contributing

Contributions are welcome — please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Commit your changes and push the branch
4. Open a pull request describing your change

Please include tests and update the README when adding new functionality.

---

## License

Add a LICENSE file to the repository and specify the license here (for example, MIT).

---

## Maintainer / Contact

Maintainer: Bhumika — https://github.com/Bhumika-1403

If you have questions or suggestions, open an issue or submit a pull request.
