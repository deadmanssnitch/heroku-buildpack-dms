# Heroku Buildpack for Dead Man's Snitch
<img src="https://d1fqvroa87ty8i.cloudfront.net/assets/field-agent-logo-59110ceeef2d9621519040fbc0b44d4690ae73b402f536cd9603592baabecc60.svg" width="180px" align="right"></img>

[Dead Man's Snitch](https://deadmanssnitch.com) is a monitoring service for
Heroku Scheduler, cron jobs, and other scheduled tasks. We let you know when your
jobs fail or they fail to run.

Field Agent is our custom monitoring agent that makes it easy to add error
alerting and gain access to the logs when a job fails.

## Getting Started

To get started, add this buildpack to the Heroku application you plan to
monitor. You will then need to build a new slug by pushing a change to the
application. Changing configuration or restarting the dynos is not enough to
trigger a new slug to be built, you will need to push a new commit.

```console
$ heroku buildpacks:add https://github.com/deadmanssnitch/heroku-buildpack-dms
```

## Setup Dead Man's Snitch

<img src="/doc/dms_heroku.png" width="460px" align="left"></img>
If you haven't already, you will want to add Dead Man's Snitch as an add-on to
your application. The free plan offers basic alerting for when jobs fail to run.

```console
heroku addons:create deadmanssnitch
```

Next, open the Dead Man's Snitch interface, enter your name and the email
address where alerts should be sent, then create your first Snitch. Give it a
name that describes what you will be monitoring.

```console
heroku addons:open deadmanssnitch
```

To get access to error alerting and failure log output you will need to upgrade
to our Surveillance Van plan

```console
heroku addons:upgrade deadmanssnitch:surveillance-van
```

## Configure Heroku Scheduler

<img src="/doc/heroku_scheduler_dms.png" width="490px" align="right"></img>
```console
$ heroku addons:open scheduler
```
This buildpack provides a new command (`dms`) that will be added to PATH that
you will use to help run your jobs. It wraps your jobs, gathers metrics and
logs, then radios the information back to Dead Man's Snitch.

You will need the token of the Snitch you created earlier. You prepend `dms`
and the token before your job in Heroku Scheduler.

For example, if your job was `rake stripe:update` and your Snitch's token was
`c2354d53d2` the<br> command would become:

```
dms c2354d53d2 rake stripe:update
```
