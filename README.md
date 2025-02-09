# Adopt-a-Drain

Claim responsibility for cleaning out a storm drain after it rains.

## Production

https://adopt-a-drain-savannah.org

## Installation
This application requires [Postgres](http://www.postgresql.org/) to be installed

    git clone git://github.com/opensavannah/adopt-a-drain.git
    cd adopt-a-drain
    bundle install

    bundle exec rake db:create
    bundle exec rake db:schema:load

See the [wiki](https://github.com/sfbrigade/adopt-a-drain/wiki/Windows-Development-Environment) for a guide on how to install this application on Windows.

## Docker
If you would rather not install RUBY and Postgres db on your local computer, you may use DOCKER to run and improve this project
To setup a local development environment with
[Docker](https://docs.docker.com/engine/installation/).   

Basic instructions for install DOCKER and this project:
1. Install Docker Toolbox  https://docs.docker.com/toolbox/toolbox_install_windows/
2. Install VirtualBox 6.0.10 (solves some bugs with the version that comes with Toolbox)
3. git clone git://github.com/opensavannah/adopt-a-drain.git

```
To run project in DOCKER:
1. Run As Adminitrator Docker QuickStart Terminal
2. Navigate to the location of this github repository locally. For Example: cd /c/Users/rachel/Documents/GitHub/adopt-a-drain-savannah.org
3. Run the following commands in the terminal
    # Override database settings as the docker host:
    echo "DB_HOST=db" > .env
    echo "DB_USER=postgres" >> .env

    # Setup your docker based postgres database:
    docker-compose run --rm web bundle exec rake db:setup

    # Load data:
    docker-compose run --rm web bundle exec rake data:load_drains
    # OR: don't load all that data, and load the seed data:
    # docker-compose run --rm web bundle exec rake db:seed

    # Start the web server:
    docker-compose up

# Your project should be up and running. Instead of http://localhost:3000 or tcp://0.0.0.0:3000 as the terminal suggests, grab the IP address of the docker container and use that.  The easiest way is to look in the Kitematic app.  Open it, select the proper container (*web) and then look at the Settings->Hostname/Ports page. Use the IP address shown with port 3000 appended, i.e. 192.168.99.3:3000.  Visit your website!
```

## Usage
    rails server

## Seed Data
    bundle exec rake data:load_drains

## Deploying to Heroku
A successful deployment to Heroku requires a few setup steps:

1. Generate a new secret token:

    ```
    rake secret
    ```

2. Set the token on Heroku:

    ```
    heroku config:set SECRET_TOKEN=the_token_you_generated
    ```

3. [Precompile your assets](https://devcenter.heroku.com/articles/rails3x-asset-pipeline-cedar)

    ```
    RAILS_ENV=production bundle exec rake assets:precompile

    git add public/assets

    git commit -m "vendor compiled assets"
    ```

4. Add a production database to config/database.yml

5. Seed the production db:

    `heroku run bundle exec rake db:seed`

If you choose, you can use the pre-defined Docker build and deployment for Heroku. 
You will need set your Heroku stack to container:

    heroku stack:set container

    git push heroku master


Keep in mind that the Heroku free Postgres plan only allows up to 10,000 rows,
so if your city has more than 10,000 fire drains (or other thing to be
adopted), you will need to upgrade to the $9/month plan.

### Google Maps API Service
You will need to apply for a Google Maps Javascript API key in order to remove the "Development Only" watermark on maps. 
After you have obtained the key, you will need to set it as an environment variable.

    heroku config:set GOOGLE_MAPS_KEY=your_maps_api_key

### Google Analytics
If you have a Google Analytics account you want to use to track visits to your
deployment of this app, just set your ID and your domain name as environment
variables:

    heroku config:set GOOGLE_ANALYTICS_ID=your_id
    heroku config:set GOOGLE_ANALYTICS_DOMAIN=your_domain_name

An example ID is `UA-12345678-9`, and an example domain is `adoptadrain.org`.

## Contributing
In the spirit of [free software][free-sw], **everyone** is encouraged to help
improve this project.

[free-sw]: http://www.fsf.org/licensing/essays/free-sw.html

Here are some ways *you* can contribute:

* by using alpha, beta, and prerelease versions
* by reporting bugs
* by suggesting new features
* by [translating to a new language][locales]
* by writing or editing documentation
* by writing specifications
* by writing code (**no patch is too small**: fix typos, add comments, clean up
  inconsistent whitespace)
* by refactoring code
* by closing [issues][]
* by reviewing patches
* [financially][]

[locales]: https://github.com/opensavannah/adopt-a-drain/tree/master/config/locales
[issues]: https://github.com/opensavannah/adopt-a-drain/issues
[financially]: https://secure.sfbrigade.org/page/contribute

## Submitting an Issue
We use the [GitHub issue tracker][issues] to track bugs and features. Before
submitting a bug report or feature request, check to make sure it hasn't
already been submitted. When submitting a bug report, please include a [Gist][]
that includes a stack trace and any details that may be necessary to reproduce
the bug, including your gem version, Ruby version, and operating system.
Ideally, a bug report should include a pull request with failing specs.

[gist]: https://gist.github.com/

## Submitting a Pull Request
1. [Fork the repository.][fork]
2. [Create a topic branch.][branch]
3. Add specs for your unimplemented feature or bug fix.
4. Run `bundle exec rake test`. If your specs pass, return to step 3.
5. Implement your feature or bug fix.
6. Run `bundle exec rake test`. If your specs fail, return to step 5.
7. Run `open coverage/index.html`. If your changes are not completely covered
   by your tests, return to step 3.
8. Add, commit, and push your changes.
9. [Submit a pull request.][pr]

[fork]: http://help.github.com/fork-a-repo/
[branch]: http://learn.github.com/p/branching.html
[pr]: http://help.github.com/send-pull-requests/

## Supported Ruby Version
This library aims to support and is [tested against][travis] Ruby version 2.2.2.

If something doesn't work on this version, it should be considered a bug.

This library may inadvertently work (or seem to work) on other Ruby
implementations, however support will only be provided for the version above.

If you would like this library to support another Ruby version, you may
volunteer to be a maintainer. Being a maintainer entails making sure all tests
run and pass on that implementation. When something breaks on your
implementation, you will be personally responsible for providing patches in a
timely fashion. If critical issues for a particular implementation exist at the
time of a major release, support for that Ruby version may be dropped.

## Copyright
Copyright (c) 2019 Code for Savannah. See [LICENSE.md](https://github.com/opensavannah/adopt-a-drain/blob/master/LICENSE.md) for details.

[license]: https://github.com/opensavannah/adopt-a-drain/blob/master/LICENSE.md


