# Django/PostgreSQL Docker Starter

This repo contains the Docker files to start a Django/PostgreSQL project. 

# Installation

Fork this repo to a new repo and run the following commands.
```bash
git clone <git-url> new_project
```

# Usage

Create a new Django project logically

```bash 
cd new_project
django-admin startproject <project-name> .
```

Configure Django settings

```python
# settings.py
import os

ALLOWED_HOSTS = [*]

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_NAME'),
        'USER': os.environ.get('POSTGRES_USER'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

Build and run containers
```bash 
docker-compose up
```

The project should be running at http://localhost:8000/