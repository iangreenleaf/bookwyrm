web: gunicorn bookwyrm.wsgi:application
scheduler: celery -A celerywyrm beat -l INFO --scheduler django_celery_beat.schedulers:DatabaseScheduler
worker: celery -A celerywyrm worker -l info -Q high_priority,medium_priority,low_priority,streams,images,suggested_users,email,connectors,lists,inbox,imports,import_triggered,broadcast,misc
release: python3 manage.py migrate && python3 manage.py compile_themes && python3 manage.py collectstatic --no-input
