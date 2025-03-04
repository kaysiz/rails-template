# mattbrictson/rails-template

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/mattbrictson/rails-template/ci.yml)](https://github.com/mattbrictson/rails-template/actions/workflows/ci.yml)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/mattbrictson/rails-template/ci-vite.yml?label=vite+build)](https://github.com/mattbrictson/rails-template/actions/workflows/ci-vite.yml)


## About

This is the application template that I recommend for Rails 7 projects. I've assembled this template over the years to include best-practices, tweaks, documentation, and personal preferences, while still generally adhering to the "Rails way".

I've written several blog posts on techniques, helpers, and boilerplate that are included in this template, if you'd like to take a deeper dive:

- [How to organize CSS in a Rails project](https://mattbrictson.com/blog/organizing-css-in-rails)
- [Tips for writing Rails tasks with Thor instead of Rake](https://mattbrictson.com/blog/rails-tasks-with-thor)
- [Automatically open the browser when the Rails server starts](https://mattbrictson.com/blog/open-browser-on-rails-start)
- [Configuring RuboCop to scan the right files in a Rails project](https://mattbrictson.com/blog/including-and-excluding-files-in-rubocop)
- [Easier Nested Layouts in Rails](https://mattbrictson.com/blog/easier-nested-layouts-in-rails)
- [Fixing slow, flaky system tests in Vite-Rails](https://mattbrictson.com/blog/faster-vite-test-without-autobuild)
- [The 3 Vite plugins I use on every new Rails project](https://mattbrictson.com/blog/3-vite-rails-plugins)
- [Inline SVGs with Rails and Vite](https://mattbrictson.com/blog/inline-svg-with-vite-rails)

## Requirements

This template currently requires:

* **Rails 7.0**
* **Ruby 3.1 or newer**
* PostgreSQL
* Node 16.14+ or 18+, and Yarn 1.x

For older versions of Rails, you can use one of [these branches](https://github.com/mattbrictson/rails-template/branches/all) going back all the way to Rails 4.2.

## Installation

*Optional.*

To make this the default Rails application template on your system, create a `~/.railsrc` file with these contents:

```
-d postgresql
-m https://raw.githubusercontent.com/mattbrictson/rails-template/main/template.rb
```

## Usage

This template assumes you will store your project in a remote git repository (e.g. GitHub) and that you will deploy to a production environment. It will prompt you for this information in order to pre-configure your app, so be ready to provide:

1. The git URL of your (freshly created and empty) GitHub repository
2. The hostname of your production server

To generate a Rails application using this template, pass the `-m` option to `rails new`, like this:

```
rails new blog \
  -d postgresql \
  -m https://raw.githubusercontent.com/mattbrictson/rails-template/main/template.rb
```

*Remember that options must go after the name of the application.* The only database supported by this template is `postgresql`.

If you’ve installed this template as your default (using `~/.railsrc` as described above), then all you have to do is run:

```
rails new blog
```

## What does it do?

The template will perform the following steps:

1. Generate your application files and directories
2. Create the development and test databases
3. Commit everything to git
4. Push the project to the remote git repository you specified

## What is included?

#### Optional support for `vite_rails`⚡️

Add the `--javascript vite` option to the `rails new` command to get started with Vite! [Vite][vite] is an easy to use alternative to Webpack(er), and much more powerful than the standard import map and css/jsbundling-rails options that are built into Rails.

- Frontend code (JS, CSS, images) will be placed in `app/frontend/`
- Run `yarn start` to start the development server with hot reloading
- Plain CSS with [modern-normalize](https://github.com/sindresorhus/modern-normalize) will be used for styles (the `--css` option will be ignored)

If you don't specify `--javascript vite`, then this template will use the standard Rails 7 behavior.

#### These gems are added to the standard Rails stack

* Core
    * [active_type][] – for building simple and effective form/service objects
    * [sidekiq][] – Redis-based job queue implementation for Active Job
* Configuration
    * [dotenv][] – for local configuration
* Utilities
    * [annotate][] – auto-generates schema documentation
    * [good_migrations][] - prevents app models from being improperly referenced in migrations
* Linting
    * [rubocop][] – enforces Ruby code style
    * [erblint][] – applies rubocop rules within html.erb files
    * [stylelint][] – checks (S)CSS files
    * [eslint][] – checks JS/TS files
* Security
    * [brakeman][] and [bundler-audit][] – detect security vulnerabilities
* Testing
    * [capybara-lockstep][] – for more reliable browser testing
    * [factory_bot_rails][] – for easy setup of test data
    * [shoulda][] – shortcuts for common ActiveRecord tests

#### Postmark

I like to use [Postmark][] for transactional email, and so I've included the [postmark-rails][] gem and configured it in `environments/production.rb`. Make sure to sign up for a Postmark account to get an API key, or switch to your own preferred email provider before deploying your app.

#### Other tweaks that patch over some Rails shortcomings

* A much-improved `bin/setup` script
* Log rotation so that development and test Rails logs don’t grow out of control

## How does it work?

This project works by hooking into the standard Rails [application templates][] system, with some caveats. The entry point is the [template.rb][] file in the root of this repository.

Normally, Rails only allows a single file to be specified as an application template (i.e. using the `-m <URL>` option). To work around this limitation, the first step this template performs is a `git clone` of the `mattbrictson/rails-template` repository to a local temporary directory.

This temporary directory is then added to the `source_paths` of the Rails generator system, allowing all of its ERb templates and files to be referenced when the application template script is evaluated.

Rails generators are very lightly documented; what you’ll find is that most of the heavy lifting is done by [Thor][]. The most common methods used by this template are Thor’s `copy_file`, `template`, and `gsub_file`. You can dig into the well-organized and well-documented [Thor source code][thor] to learn more.

[active_type]:https://github.com/makandra/active_type
[sidekiq]:http://sidekiq.org
[dotenv]:https://github.com/bkeepers/dotenv
[annotate]:https://github.com/ctran/annotate_models
[rubocop]:https://github.com/bbatsov/rubocop
[erblint]:https://github.com/Shopify/erb-lint
[factory_bot_rails]:https://github.com/thoughtbot/factory_bot_rails
[Postmark]:http://postmarkapp.com
[postmark-rails]:http://www.rubydoc.info/gems/postmark-rails/0.12.0
[brakeman]:https://github.com/presidentbeef/brakeman
[bundler-audit]:https://github.com/rubysec/bundler-audit
[shoulda]:https://github.com/thoughtbot/shoulda
[application templates]:http://guides.rubyonrails.org/generators.html#application-templates
[template.rb]: template.rb
[thor]: https://github.com/rails/thor
[vite]: https://vite-ruby.netlify.app
[good_migrations]: https://github.com/testdouble/good-migrations
[capybara-lockstep]: https://github.com/makandra/capybara-lockstep
[eslint]: https://eslint.org
[stylelint]: https://stylelint.io
