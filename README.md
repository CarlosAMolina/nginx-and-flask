## Introduction

Project to learn how to run a Flask app in a nginx server.

## Run localhost

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

Open in web browser:

<http://0.0.0.0:8081/>

## References

Tutorial

<https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04>
