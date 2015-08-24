# Heroku Buildpack for Node.js

![nodesjs](https://cloud.githubusercontent.com/assets/51578/8882955/3f0c3980-3219-11e5-8666-bc9c926a7356.jpg)


This is the official [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Node.js apps. If you fork this repository, please **update this README** to explain what your fork does and why it's special.

## Documentation

For more information about using Node.js and buildpacks on Heroku, see these Dev Center articles:

- [Heroku Node.js Support](https://devcenter.heroku.com/articles/nodejs-support)
- [Getting Started with Node.js on Heroku](https://devcenter.heroku.com/articles/nodejs)
- [10 Habits of a Happy Node Hacker](https://blog.heroku.com/archives/2014/3/11/node-habits)
- [Buildpacks](https://devcenter.heroku.com/articles/buildpacks)
- [Buildpack API](https://devcenter.heroku.com/articles/buildpack-api)


What this buildpack does
========================

 * Run `npm install`
 * Install latest version of `gulp` if a gulpfile is present and was not already installed by previous pass
 * Install latest version of `bower` if a bowerfile is present and was not already installed by previous pass
 * Install latest version of `grunt` if a gruntfile is present and was not already installed by previous pass
 * Install `bundle`
 * Run `bower install`
 * Run `bundle install`
 * Run `bundle exec compass compile`
 * Run `gulp heroku:$NODE_ENV` (by default, it expands to `heroku:production`)
 * Run `grunt heroku:$NODE_ENV`

Usage
-----

Create a new app with this buildpack:

    heroku create myapp --buildpack heroku config:add BUILDPACK_URL=https://github.com/stephanmelzer/heroku-buildpack-nodejs-grunt-compass.git

Or add this buildpack to your current app:

    heroku config:add BUILDPACK_URL=https://github.com/stephanmelzer/heroku-buildpack-nodejs-grunt-compass.git

Set the `NODE_ENV` environment variable (e.g. `development` or `production`):

    heroku config:set NODE_ENV=production

Create your Node.js app and add a Gruntfile named  `Gruntfile.js` (or `Gruntfile.coffee` if you want to use CoffeeScript, or `grunt.js` if you are using Grunt 0.3) with a `heroku` task:

    grunt.registerTask('heroku:development', 'clean less mincss');

or

    grunt.registerTask('heroku:production', 'clean less mincss uglify');


How to access private repos
---------------------------
Currently we support private repos for bower, for GitHub, by leveraging the "shorthand syntax". Follow this guide:

 * In your bower.json, install private repos using the syntax "organization/repo".
 * Create or modify your `.bowerrc` file, adding this line: `"shorthand-resolver": "git@github.com:{{owner}}/{{package}}.git"`. This tells bower to normally access GitHub using SSH, so that you have access to your private repos.
 * Create a personal access token from your [GitHub account security page](https://github.com/settings/applications). Unfortunately, GitHub doesn't allow to limit the token to access to a specific repo (nor a specific organization). If you feel uncomfortable with this, you need to create a new dummy GitHub user, grant it access to the repo(s) that you need to access from Heroku, and then create a personal access token for this user.
 * Save the token in the Heroku enviornment as `GITHUB_AUTH_TOKEN`.

The buildpack will then automatically instruct bower to access your repos through the token instead of using SSH (for which it wouldn't have access).


