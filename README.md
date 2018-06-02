# Travi CI Everything

[Travis CI](https://travis-ci.org/) configuration snippet list. copy, paste, code :-)

## Motivation

Automation is awesome. I use Travis CI in almost all my open-source projects for linting, testing, deployment, etc. Whenever a new project is started, I copy the CI configuration from my other projects for common tasks. Compared to travel around existing projects, having a list for frequently used CI configuration snippet will make the search much simpler.

## Contribution

Contribution is definitely welcomed!

Note that this list is for general use of Travis CI. Its aim is to provide a quick checklist and people can copy-and-paste with minimal change. To show the real-world use case, when submitting a new snippet, it is encourage to attach a link to the project that is using this snippet.

Since I am a JavaScript developer, this list is mostly based on a [Node.js](https://nodejs.org/en/) project.

## Testing

### General Case

Example: [w3c-dcat](https://github.com/haoliangyu/w3c-dcat/blob/master/.travis.yml)

``` yaml
language: node_js
node_js:
  - "node"
script:
  - npm test
```

### Test with PostgreSQL

Example: [pg-reactive](https://github.com/haoliangyu/pg-reactive/blob/master/.travis.yml)

``` yaml
language: node_js
node_js:
  - "node"
services:
  # install PostgreSQL
  - postgresql
before_script:
  # a script to create the test database
  - psql -c 'create database travis_ci_test;' -U postgres
  # a script to prepare the test database, like setting up the schema
  - npm run setup-db
script:
  - npm test
```

## Deployment

### GitHub Pages

Example: [ngx-leaflet-starter](https://github.com/haoliangyu/ngx-leaflet-starter/blob/master/.travis.yml)

Environment Variables:
* **GITHUB_TOKEN**: your [GitHub token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) to update the `gh-pages` branch

``` yaml
language: node_js
node_js:
  - "node"
script:
  # build the project first
  - npm run build
deploy:
  # target is github page
  provider: pages
  # read the page content from the /dist folder
  local-dir: dist
  # leave the build files unchanged
  skip-cleanup: true
  # insert your github token
  github-token: $GITHUB_TOKEN
  # deploy when the master branch is changed
  on:
    branch: master
```

### AWS S3

Example: [SingularData/data-source](https://github.com/SingularData/data-source/blob/master/.travis.yml)

Environment Variables:
* **AWS_KEY_ID**: your AWS access key
* **AWS_SECRET_ACCESS_KEY**: your AWS secret access key

``` yaml
language: node_js
node_js:
  - "node"
script:
  # build the project first
  - npm run build
deploy:
  # target is AWS S3
  provider: s3
  # AWS access key
  access_key_id: $AWS_KEY_ID
  # AWS secret access key
  secret_access_key: $AWS_SECRET_ACCESS_KEY
  # bucket name
  bucket: my_bucket
  # bucket region
  region: us-east-1
  # read the data from the /dist folder
  local_dir: dist
  # leave the build files unchanged
  skip_cleanup: true
  # deploy when the master branch is changed
  on:
    branch: master
```

### Scripted

Example: [ecs-auto-deploy](https://github.com/haoliangyu/ecs-auto-deploy/blob/master/.travis.yml)

``` yaml
language: node_js
node_js:
  - "node"
deploy:
  # use scripted deployment
  provider: script
  # use the script
  script: bash deploy.sh
  # deploy when the master branch is changed
  on:
    branch: master
```
