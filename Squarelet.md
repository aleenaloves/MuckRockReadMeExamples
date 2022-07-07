
# Squarelet

User account service for MuckRock and DocumentCloud.

Contents

 - [Install](#install)
 - [Quickstart](#quickstart)
 - [Integrations with MuckRock and DocumentCloud](#integrations)
 - [Docker Containers Info](#docker-containers-info)
 - [Invoke Info](#invoke-info)
 - [Pip Tools](#pip-tools)

## Install

### Software required

1. [docker][docker-install]
2. [docker-compose][docker-compose-install]
3. [python][python-install]
4. [invoke][invoke-install]
5. [mkcert][mkcert-install]

### Installation Steps

Check out the git repository.

```bash
git clone https://github.com/MuckRock/squarelet.git
```

Enter the directory.

```bash
cd squarelet
```

Run the environment initialization script, which will create files with the environment variables needed to run the development environment.

```bash
python initialize_dotenvs.py
```

Set an environment variable that directs `docker-compose` to use the `local.yml` file.

```bash
export COMPOSE_FILE=local.yml
```

Generate local certificates for SSL support.

```bash
inv mkcert
```

Start the docker containers. This will build and start all of the Squarelet session docker images using docker-compose.  It will bind to port 80 on localhost, so you must not have anything else running on port 80.

```bash
inv up
```
Set `dev.squarelet.com` to point to localhost.
```bash
sudo echo "127.0.0.1   dev.squarelet.com" >> /etc/hosts
```

Enter `dev.squarelet.com` into your browser. You should see the Muckrock Squarelet home page.

If you are developing on any of our other projects. Follow their respective integration steps. 

 - MuckRock (should link to a Squarelet section in MuckRock)
 - DocumentCloud (should link to a Squarelet section in DocumentCloud)

[docker]: https://docs.docker.com/
[docker-compose]: https://docs.docker.com/compose/
[nginx]: https://www.nginx.com/
[mailhog]: https://github.com/mailhog/MailHog
[django]: https://www.djangoproject.com/
[postgres]: https://www.postgresql.org/
[redis]: https://redis.io/
[celery]: https://docs.celeryproject.org/en/latest/
[invoke]: http://www.pyinvoke.org/
[docker-install]: https://docs.docker.com/install/
[docker-compose-install]: https://docs.docker.com/compose/install/
[invoke-install]: http://www.pyinvoke.org/installing.html
[python-install]: https://www.python.org/downloads/
[codeship]: https://app.codeship.com/projects/296009
[pylint]:  https://www.pylint.org/
[black]: https://github.com/psf/black
[pip-tools]: https://github.com/jazzband/pip-tools
[mkcert-install]: https://github.com/FiloSottile/mkcert#installation
