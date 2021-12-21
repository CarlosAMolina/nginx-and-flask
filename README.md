## Table of contents

- [Introduction](#introduction)
- [Run](#run)
  - [Localhost](#localhost)
  - [VPS with HTTPs](#vps-with-https)
- [References](#references)

## Introduction

Project to learn how to run a Flask app in a nginx server.

## Run

### Localhost

Create file: 

```bash
sudo vi /etc/nginx/sites-available/test.es
```

File content (update the `CORRECT_PATH` value):

```bash
server {
    listen       8081;
    server_name  localhost;

    location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://unix:/home/CORRECT_PATH/nginx-and-flask/myproject.sock;
    }
}
```

Create link:

```bash
sudo ln -s /etc/nginx/sites-available/test.es /etc/nginx/sites-enabled
```

Run Python app:

```bash
gunicorn --workers 3 --bind unix:myproject.sock wsgi:app
```

Start nginx server:

```bash
sudo nginx
```

Open in  the web browser:

<http://0.0.0.0:8081/>

### VPS with HTTPs

Warning. This is a quick way to run the app, but probably not recommended.

Add to the site's configuration:

```bash
   location /test {
        proxy_pass 'http://localhost:5000/';
    }
```

Reload nginx and run the app:

```bash
python myproject.py 
```

Open in the web browser:

<https://DOMAIN/test>


## References

Tutorial

<https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04>
