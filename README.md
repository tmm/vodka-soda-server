# vodka-soda-server

> Casual without compromise

## Set Up Local Dev Environment

Install tools and dependencies (via [homebrew](https://brew.sh/)):

```bash
> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
> brew tap homebrew/bundle
> brew bundle
```

Next, create a local Postgres database:

1. Open `postgres.app` (just installed via `brew`)
2. Open `postico.app`
3. Create a database through `positico`

Add environment variables:

```bash
export DJ_ENV=dev
export DJ_DB_NAME=vodka-soda # database you just created
export DJ_DB_USER=tom # this is likely the user you use to login to your computer
export DJ_DB_PASSWORD=root
export DJ_DB_HOST=docker.for.mac.localhost # special docker set up
export DJ_DB_PORT=5432
export DJ_SECRET_KEY=+cg9iso$a55f3ay&)pdg3k&=lq_c*55j7oyuib=a(pi#2$oj^0
export DJ_SOCIAL_AUTH_FACEBOOK_KEY=234928371610299
export DJ_SOCIAL_AUTH_FACEBOOK_SECRET=3731939d837s7b1b0b772a64c7570edb
```

Snag the repo, start Docker, and build the containers:

```bash
> git clone https://github.com/tmm/vodka-soda-server.git
> cd vodka-soda-server
> docker-compose build
```

Apply database migrations, create superuser, and generate static files (for admin console, etc.):

```bash
> docker-compose run django python manage.py makemigrations
> docker-compose run django python manage.py migrate
> docker-compose run django python manage.py collectstatic
```

If everything worked as planned, you should be able to `docker-compose up` and access the admin app.

```bash
> docker-compose up -d
> docker-compose ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED            STATUS            PORTS                NAMES
13cf068eefeb        vodkasodaserver_nginx    "nginx -g 'daemon of…"   1 minutes ago      Up 1 minute       0.0.0.0:80->80/tcp   nginx
f2bc789461bf        vodkasodaserver_django   "uwsgi --ini uwsgi.i…"   1 minutes ago      Up 1 minute       8000/tcp             django
```

Go to the [admin site](http://0.0.0.0/admin/oauth2_provider/application/add/) to create a new `Application` for the API to connect to.

## Creating/Updating DB Models

Whenever you make changes to a model, the database needs to be kept in sync.

```bash
> docker-compose run django python manage.py makemigrations
> docker-compose run django python manage.py migrate
```

See [Django Migrations Worflow](https://docs.djangoproject.com/en/2.0/topics/migrations/#workflow) for more info.

## Deploying to production

TBD

Add additional environment variables:

```bash
export AWS_ACCESS_KEY_ID=AMAEPJCKIAIPOZTEM74G
export AWS_SECRET_ACCESS_KEY=C9Y3GI46LrBMnT/jJCsE561cEiAbWn/iU1FPw9hx
```

## Helpful links

+ [Docker for Beginners](https://docker-curriculum.com)
+ [Django OAuth Toolkit with Django REST Framework Tutorial](https://django-oauth-toolkit.readthedocs.io/en/latest/rest-framework/rest-framework.html)
+ [Django REST Framework Social Oauth2 Facebook Example](https://github.com/PhilipGarnero/django-rest-framework-social-oauth2#facebook-example)
+ [dry-rest-permissions](https://github.com/dbkaplan/dry-rest-permissions)
+ [GeoDjango](https://docs.djangoproject.com/en/2.0/ref/contrib/gis/)
+ [Facebook Developer Access Tokens](https://developers.facebook.com/tools/accesstoken/)
+ [Facebook App Dashboard](https://developers.facebook.com/apps/234319953810299/dashboard/)
+ [Tinder API](https://gist.github.com/rtt/10403467)
