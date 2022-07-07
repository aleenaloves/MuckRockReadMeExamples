# MuckRock
[![Codeship Status for MuckRock/muckrock][codeship-img]][codeship]
[![codecov.io][codecov-img]][codecov]

MuckRock is a non-profit collaborative news site that gives you the tools to keep our government transparent and accountable.

## Prerequisites 
You must first have this up and running: [MuckRock/squarelet: MuckRock User Service (github.com)](https://github.com/muckrock/squarelet)

## Install

### Software required

1. [docker][docker-install]
2. [docker-compose][docker-compose-install]
3. [python][python-install]
4. [invoke][invoke-install]

### Squarelet Integration
The Squarelet project provides an authentication system for MuckRock. Therefore, there must be certain things set up within Squarelet before you can begin accessing them individually. 

 1. With the Squarelet containers running, in your terminal within the Squarelet folder, open up the bash shell with `inv sh`
 2. Within the bash shell, utilize this command to create your RSA key `./manage.py creatersakey`
 3. Then create a superuser utilizing the following command: `./manage.py createsuperuser`. 
 4. Exit the bash shell with the command `exit`
 5. Enter the Django shell with the command `inv shell`
 6. Grab the user you have just created with the following command
```python
tempUser = User.objects.all()[0]
```  
*If you created multiple users you will have to use Django queries to grab the exact user you want.*

 7. Then set the `tempUser` variable's `is_staff` value equal to true, and save the user. 
 ```python
tempUser.is_staff = True
```  
8. Exit the Django shell with the command `exit`
9. In a browser navigate to the admin portal. You can find this portal by using the base Squarelet URL, and appending /admin to the end of it.
10. Using the credentials you just created, login. *If you find yourself being redirected to the login page on successful credentials, change the browser you are using.* 
11. Navigate to [Clients](https://dev.squarelet.com/admin/oidc_provider/client/)
12. Create a client called `MuckRock Dev`
13. Make sure the fields have the following values:

|Field| Value |
|--|--|
| Owner | blank/your user account |
|Client Type|Confidential|
|Response Types|code (Authorization Code Flow)|
|Redirect URIs (on separate lines)|http://dev.muckrock.com/accounts/complete/squarelet http://dev.foiamachine.org/accounts/complete/squarelet|
|JWT Algorithm|RS256|
|Require Consent?|Unchecked|
|Reuse Consent|Checked|
|Client ID|This will be filled in automatically upon saving|
|Client SECRET|This will be filled in automatically upon saving|
|Scopes (on separate lines)|read_user write_user read_organization write_charge read_auth_token|
|Post Logout Redirect URIs (on separate lines)|http://dev.muckrock.com/ http://dev.foiamachine.org/|
|Webhook URL (To make this field appear, Add a client profile)|http://dev.muckrock.com/squarelet/webhook/|
14. Click save and continue editing. Note down the `Client ID` and `Client SECRET` values. You will need these later.

### Installation Steps

MuckRock depends on Squarelet for user authentication.  As the services need to communivate directly, the development environment for MuckRock depends on the development environment for Squarelet - the MuckRock docker containers will join Squarelet's docker network.  [Please install Squarelet and set up its development environment first][squarelet].

1. Check out the git repository - `git clone git@github.com:MuckRock/muckrock.git`
2. Enter the directory - `cd muckrock`
3. Run the dotenv initialization script - `python initialize_dotenvs.py`
This will create files with the environment variables needed to run the development environment.
4. Set up the javascript run `inv npm "install"` and `inv npm "run build"`
5. Start the docker images - `inv up`
This will build and start all of the docker images using docker-compose.  The invoke tasks specify the `local.yml` configuration file for docker-compose.  If you would like to run docker-compose commands directly, set the environment variable `export COMPOSE_FILE=local.yml`.
6. Set `dev.muckrock.com` to point to localhost - `sudo echo "127.0.0.1   dev.muckrock.com" >> /etc/hosts`
7. Enter `dev.muckrock.com` into your browser - you should see the MuckRock home page.
8. In  `.envs/.local/.django` (**in the MuckRock project**) set the following environment variables:

-   `SQUARELET_KEY`  to the value of Client ID
-   `SQUARELET_SECRET`  to the value of Client SECRET

You should now be able to log in to MuckRock using your Squarelet account.

[docker]: https://docs.docker.com/
[docker-compose]: https://docs.docker.com/compose/
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
[pip-tools]: https://github.com/jazzband/pip-tools
[codeship]: https://codeship.com/projects/52228
[codeship-img]: https://codeship.com/projects/c14392c0-630c-0132-1e4c-4ad47cf4b99f/status?branch=master
[codecov]: https://codecov.io/github/MuckRock/muckrock?branch=master
[codecov-img]:https://codecov.io/github/MuckRock/muckrock/coverage.svg?token=SBg37XM3j1&branch=master
[squarelet]: https://github.com/muckrock/squarelet/
[yapf]: https://github.com/google/yapf
[watson]: https://github.com/etianen/django-watson
