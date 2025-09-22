1. Install Python and Postgres and Redis
1. Set hosts in .env to localhost
1. `pip install -r requirements.txt`
1. Create DB:
    postgres=# CREATE USER bookwyrm WITH ENCRYPTED PASSWORD 'mypassword';
    postgres=# CREATE DATABASE bookwyrm OWNER bookwyrm;
    postgres=# ALTER USER bookwyrm CREATEDB;
1. `python manage.py migrate`
1. `python manage.py migrate django_celery_beat`
1. `python manage.py initdb`
1. `python manage.py compile_themes`
1. `python manage.py collectstatic --no-input`
1. `python manage.py admin_code`
1. `gunicorn bookwyrm.wsgi:application`
