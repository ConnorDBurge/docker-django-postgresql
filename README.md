# Django/PostgreSQL Docker Boilerplate

# Installation
Fork this repo and run the following commands

# Set-up
Clone new repo
```bash
git clone <repo-url>
```
Start a new project locally. This project will be mounted as a volume.
```bash
django-admin startproject <project-name>
```
Configure Django settings
```python
# settings.py
import os

ALLOWED_HOSTS = ["*"]

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
Double check docker-compose is configured correctly.
```yml
version: "3.9"

services:
  db:
    image: postgres
    volumes:
      - ./postgresdata:/var/lib/postgresql/data
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/usr/src/app/
    ports:
      - "8000:8000"
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    depends_on:
      - db

volumes:
  postgresdata:
```
The file structure should look like this:
```bash
|--postgresdata
|--<project-name>
|--.gitignore
|--docker-compose.yml
|--Dockefile
|--manage.py
|--README.md
|--requirements.txt
```

# Run
```bash
docker-compose up -d
```

# Executing commands
Entering the CLI
```bash
$ docker exec -it <container-id> /bin/sh

$ /usr/src/app # python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  No migrations to apply.
/usr/src/app # 
```