---
layout: default
title: Heroku & Git
parent: API
nav_order: 3
---

By Vincent Huynh

# Heroku

In case you need a more detailed/fleshed out look into Heroku, please visit [here](https://devcenter.heroku.com/articles/git#prerequisites-install-git-and-the-heroku-cli).

Otherwise, I will provide the following as a quick getting started /refresher guide.

## Git and Heroku CLI

1. This assumes you've already got Git setup on your device. If you do not, you can find information about that [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

2. Download and install the Heroku CLI. You can find that [here](https://devcenter.heroku.com/articles/heroku-cli#download-and-install).

3. Navigate to your local git repo. Make sure it has at least one commit

```
git init
git add .
git commit -m "Some commit message"
```

## Heroku Remote (preexisting Heroku App)

In case you are trying to update the pre-existing deployed Heroku app, clone the Heroku by:

```
heroku git:clone -a croppricingapi
```

Then, once you've made your changes, simply

```
git add .
git commit -m "some message"
git push heroku master
```

**Note**: If you are not pushing from the master branch, you should append `:master` to the end of your push, like so:

```
git push heroku heroku-staging:master
```

where `heroku-staging` is the branch I used before pushing to Heroku.

## Heroku Remote (Brand new Heroku app)

To deploy to Heroku, you need to setup a Heroku remote on your git repo.

1. Navigate to the local repo. Use the Heroku CLI and type

```
heroku create
```

2. Confirm that the remote was created with

```
git remote -v
```
