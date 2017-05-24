# Timestrap [![Travis](https://img.shields.io/travis/overshard/timestrap.svg?style=flat-square)](https://travis-ci.org/overshard/timestrap) [![Coveralls](https://img.shields.io/coveralls/overshard/timestrap.svg?style=flat-square)](https://coveralls.io/github/overshard/timestrap) [![license](https://img.shields.io/github/license/overshard/timestrap.svg?style=flat-square)](https://github.com/overshard/timestrap/blob/master/LICENSE.md)

Time tracking and invoicing you can host anywhere. Full export support in
multiple formats and easily extensible.

![Timestrap](screenshot.png)

### :warning: Warning

This app is currently very unstable. Everything may, and probably will, change.

## Demo

There is a [demo instance of Timestrap](https://timestrap.herokuapp.com/) on
Heroku that resets every 10 minutes. The default credentials are:

- Username: `admin`
- Password: `changeme123`

## Quickstart

Want to get up and running quickly?

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/overshard/timestrap)

The Heroku deployment has a default username and password with superuser 
access, please change it via the admin panel:

- Username: `admin`
- Password: `changeme123`

For manual deployments to Heroku without using the deploy button, make sure to
create two settings before pushing using `heroku config:set`:

    heroku config:set DJANGO_SETTINGS_MODULE=timestrap.settings.heroku
    heroku config:set SECRET_KEY=ChangeMeToSomethingRandom

After a successful push, create a super user to allow login:

    heroku run python manage.py createsuperuser

## Installation

**Important Note:** Installing Node/NPM and Chrome/Selenium are not required by
this project, Node/NPM are used for building our static files and improving our
workflow, if you aren't going to make changes to static files you don't need
them. Chrome/Selenium are specifically used for testing, if you don't want to
run all our tests you can exclude this too. All you need is any version of
Python with the virtualenv and pip packages.

For all systems you are going to need:

- Python 2.7, 3.4, 3.5, or 3.6
- Python virtualenv and pip packages

Once you have all of that you can run the following and move onto Testing
and/or Running Timestrap:

    virtualenv .venv
    source .venv/bin/activate
    pip install -r requirements/development.txt

If you'd prefer to have a minimal installation of Timestrap you can use our
base requirements instead of the development or Heroku requirements by running:

    pip install -r requirements/base.txt

If you plan on helping with front-end development you will also need Node.js.
You can then install our Node.js dependencies by running:

    sudo npm install -g gulp-cli
    npm install

### Windows

The easiest way to develop on Windows now days is using the WSL. We'll let you
figure out how to get that setup for your machine however once it is installed
most of the instructions are the exact same as the Ubuntu install instructions
after that with the exception of Google Chrome and Chromedriver.

Download and install the latest version of Google Chrome Beta and Chromedriver.
put the Chromedriver in a reasonable location, I tend to put apps that don't
come with an installer in `%LOCALAPPDATA%`. For Chromedriver I copied
`chromedriver.exe` into `%LOCALAPPDATA%\chromedriver\`.

Open up Bash on Ubuntu on Windows and run the following to make chromedriver
accessible to our tests:

    sudo mkdir -p /usr/local/bin/ && cd /usr/local/bin/
    sudo ln -s /c/mnt/Users/<YOUR USERNAME HERE>/AppData/Local/chromedriver/chromedriver.exe chromedriver

Continue on to the Ubuntu instructions to finish up and ignore the Chrome and
Chromedriver installation there.

### Ubuntu

You can install everything you need from apt, which is just virtualenv:

    sudo apt install python-virtualenv

If you are doing front-end development you also need NPM and Node.js:

    sudo apt install npm

If you want to run tests you will need to install some additional packages,
these are not required though and if you are working on small changes or
documentation then you can rely on Travis CI to run tests for you.

We've found that google-chrome-beta (Chrome 59+) is best for testing since they
have added a ton of improvements that allow for things like headless testing. We
originally used geckodriver and Firefox but Chrome finished all our tests in a
fraction of the time. So to install Chrome on Ubuntu follow the steps below.

    curl -L https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main"
    sudo apt update
    sudo apt install google-chrome-beta
    curl -L https://chromedriver.storage.googleapis.com/2.29/chromedriver_linux64.zip -o chromedriver.zip
    sudo mkdir -p /usr/local/bin/
    sudo unzip chromedriver.zip -d /usr/local/bin/
    sudo chmod +x /usr/local/bin/chromedriver
    echo "GOOGLE_CHROME_BINARY=/usr/bin/google-chrome-beta" >> ~/.bashrc

Go to the top of Installation and make sure you have virtualenv and npm all set
then continue to running and testing.

### OS X

Homebrew, get it if you don't have it and run:

    brew install node python

Make sure you have virtualenv installed after this with:

    pip install virtualenv

If you want to test with selenium install the latest Google Chrome Beta and run:

    brew install chromedriver

Go to the top of Installation and make sure you have virtualenv and npm all set
then continue to running and testing.


## Testing

We use selenium with the chromedriver for testing. If you wish to run tests you
will need to make sure you have Chrome installed. For installation instructions
on those see the above documentation

I'm trying to push for 100% code coverage on this project! If you want to add
or change something and test that everything still works you can do so easily
with:

    python manage.py test

If you push code to our primary repository we test for style adherence and code
coverage. If you get a failed build to either of these we won't accept your
code till it's fixed.

### Sauce Labs

In order to run Selenium tests in a consistent environment, this project makes
use of [SauceLabs](https://saucelabs.com/) for browser testing. To run tests on
SauceLabs, you will need to first create an account. Once you have your 
username and access key, follow the steps below:

Install the sc proxy client

    wget https://saucelabs.com/downloads/sc-4.4.6-linux.tar.gz
    tar zxf sc-4.4.6-linux.tar.gz
    mv sc-4.4.6-linux/bin/sc /usr/local/bin

From within the virtual environment

    pip install sauceclient
    export SAUCE_USERNAME=[YOUR USERNAME]
    export SAUCE_ACCESS_KEY=[YOUR ACCESS KEY]
    sc &

After "Sauce Connect is up, you may start your tests."

    python manage.py test


## Running Timestrap

Always make sure you are in the virtual environment before running additional
commands by first running `source .venv/bin/activate`. If you have already done
this from the previous step and have not left the environment continue on!

If you have not yet migrated your database do so by running:

    python manage.py migrate

You'll need to create your first user too:

    python manage.py createsuperuser

After this you can run Timestrap and access it from your browser at
`localhost:8000`.

    python manage.py runserver

### Running Timestrap With Gulp

Similar to above you will still need to be in the virtual environment by running
`source .venv/bin/acivate`. Also make sure you've migrated, created a superuser
and the other tasks. Once that is done you can then run Timestrap with Gulp:

    gulp

This provides you with live updated static files for working on the files in
`static_src`. Once you've completed your changes you can stop Gulp, run 
`gulp build` and commit the changes. We do this to reduce the
number of dependencies required to install Timestrap for people who don't want
to update static files source code or dependencies.


## Generate Fake Data

Want to see how Timestrap would look after being used a while? Run `fake` to
generate some data. Don't run this on a production database or you'll have to
do a lot of clean up.

    python manage.py fake


## Password Resets and Email

To support email for things like password resetting you need to update
Timestrap's settings. I will not presume your email situation and allow you to
do this yourself by reading [Django's documentation](https://docs.djangoproject.com/en/1.11/ref/settings/#email-backend).

If you are using Heroku you can add `sendgrid` to your apps addons on the 
Heroku admin panel or by running:

    heroku addons:create sendgrid

You then need to add these settings to `timestrap/settings/heroku.py`:

    EMAIL_HOST = 'smtp.sendgrid.net'
    EMAIL_HOST_USER = os.environ.get('SENDGRID_USERNAME')
    EMAIL_HOST_PASSWORD = os.environ.get('SENDGRID_PASSWORD')
    EMAIL_PORT = 587
    EMAIL_USE_TLS = True


## Time and Date Localization

If you wish to change things like the date strings you'll need to play around
with [Django's format localization settings](https://docs.djangoproject.com/en/1.11/topics/i18n/formatting/#controlling-localization-in-templates)
in `timestrap/settings/base.py`. We don't do anything to try and localize by
default but we are trying to avoid lock-in to a specific format. If you enable
localization and run into any bugs let us know!
