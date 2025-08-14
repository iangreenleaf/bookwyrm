On Dokku:

```
dokku apps:create bookwyrm
dokku builder:set bookwyrm selected herokuish

sudo dokku plugin:install https://github.com/dokku/dokku-redis.git redis
dokku redis:create bookwyrm-activity
dokku redis:link bookwyrm-activity bookwyrm -a REDIS_ACTIVITY
dokku redis:create bookwyrm-broker
dokku redis:link bookwyrm-broker bookwyrm -a REDIS_BROKER

dokku plugin:install https://github.com/dokku/dokku-postgres.git
dokku postgres:create bookwyrm
dokku postgres:link bookwyrm bookwyrm

dokku config:set --no-restart bookwyrm DOMAIN=bookwyrm.mydomain.net
dokku config:set --no-restart bookwyrm SECRET_KEY=my-app-secret
dokku config:set --no-restart bookwyrm EMAIL_HOST=smtp.yourdomain.com
dokku config:set --no-restart bookwyrm EMAIL_HOST_USER=mail@your.domain.here
dokku config:set --no-restart bookwyrm EMAIL_HOST_PASSWORD=abcd
dokku config:set --no-restart bookwyrm EMAIL_PORT=587
dokku config:set --no-restart bookwyrm EMAIL_USE_TLS=true
dokku config:set --no-restart bookwyrm EMAIL_USE_SSL=false
dokku config:set --no-restart bookwyrm EMAIL_SENDER_NAME=bookwyrm
dokku config:set --no-restart bookwyrm EMAIL_SENDER_DOMAIN=mydomain.net
dokku config:set --no-restart bookwyrm DISABLE_COLLECTSTATIC=1

dokku storage:ensure-directory bookwyrm --chown herokuish
dokku storage:mount bookwyrm /var/lib/dokku/data/storage/bookwyrm/images:/app/images
dokku storage:mount bookwyrm /var/lib/dokku/data/storage/bookwyrm/exports:/app/exports
dokku storage:mount bookwyrm /var/lib/dokku/data/storage/bookwyrm/static:/app/static
```

From your local machine:
```
git remote add dokku dokku@dokku.mydomain.net:bookwyrm
git push dokku production
```

On Dokku:
```
dokku ps:scale bookwyrm scheduler=1 worker=1

dokku domains:set bookwyrm bookwyrm.mydomain.net
dokku domains:report bookwyrm

dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku letsencrypt:set bookwyrm email me@example.com
dokku letsencrypt:enable bookwyrm

dokku run bookwyrm python3 manage.py admin_code

```
