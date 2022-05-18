[![Build Status](https://app.travis-ci.com/Montana/travis-nyc.svg?branch=master)](https://app.travis-ci.com/Montana/travis-nyc)

## nyc w/ Travis by Montana Mendy 

<img width="1230" alt="Screen Shot 2022-05-17 at 5 15 52 PM" src="https://user-images.githubusercontent.com/20936398/168932314-811e0aa5-6350-40c0-8e0e-5e77f63842d7.png">

## How nyc works

The `nyc` command-line-client for Istanbul works well with most JavaScript testing frameworks: tap, mocha, AVA, etc. `nyc` is a command line tool for instrumenting code with Istanbul coverage (the successor to the istanbul command line tool). For more information go to Istanbul's website: https://istanbul.js.org/integrations/ and click the `nyc` integration: 

<img width="1068" alt="Screen Shot 2022-05-17 at 5 26 50 PM" src="https://user-images.githubusercontent.com/20936398/168933210-05e6f227-d524-46e6-91ee-70e3a6ec417d.png">

Let's get onto usage.

## Usage 

First clone my repo: 

```bash
git clone https://github.com/Montana/travis-nyc.git
```

You'll see under Mongoose these three variables in `app.js`: 

```js
 useNewUrlParser: true,
 useCreateIndex: true,
 useFindAndModify: false
 ```
 
 Remove them. 
 
 ## MongoDB 
 
 Fetch MongoDB, in your `.travis.yml` file under the `script:` hook, do:
 
 ```yaml
  - wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
  - sudo apt-get install -y mongodb-org
```

You clearly see we are fetching Mongo's `apt-key`'s and then using `apt-get` to finish off the install, make sure you have `gnupg` installed as well you can do that by doing:
```bash
sudo apt-get install gnupg
```

## Installing nyc and connecting to Mongo with mock credentials

You'll want to run the following in your project root:

```bash
npm init --yes
npm i express mocha chai supertest nyc mongoose
```

Depending on your version of node, you may have to run:

```bash
npm install -g nyc
```

For `nyc` to install completely. Let's now connect our app to our mock Mongo server: 

```bash
sudo systemctl start mongo
```

Now that we are connected, let's run our app: 

```bash
node bin/www & 
```

## The `.travis.yml` file

This is how my final `.travis.yml` file came out after I was done creating it: 

```yaml   
services:
  - docker
language: node_js
node_js:
  - 17
script: 
  - npm init --yes
  - npm i express mocha chai supertest nyc mongoose
  - sudo apt-get install gnupg
  - wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
  - sudo apt-get install -y mongodb-org
  - sudo systemctl start mongod
  - node bin/www & 
  - npm install -g nyc
  - nyc npm run test
  ```
 We call on Docker for services, we are going to use Node version 17, and also start installing the `nyc` packages, and you kind of see how the rest progresses. _Important to have `&` after `node bin/www` or your build will get stuck in Travis, this tells Travis to have it run as a background process._

## Testing with nyc 

Now, time to test your project with `nyc`, lets run:

```bash
nyc npm run test
```
If it's successful you should see this in Travis: 

<img width="1208" alt="Screen Shot 2022-05-17 at 5 11 51 PM" src="https://user-images.githubusercontent.com/20936398/168932012-2e06635f-4448-4d96-bfbb-ce504949f53f.png">

If you run it locally, you can get a GUI version of what you're seeing in the Travis build, which looks like this: 

<img width="1774" alt="Screen Shot 2022-05-17 at 5 12 42 PM" src="https://user-images.githubusercontent.com/20936398/168932096-a1426ea2-68f9-47b0-b5f9-e100168c8064.png">


## Conclusion 

This is the simplest way to use `nyc` and Travis together. This was made by myself (Montana Mendy) and as per usual if you have any questions please email me at [montana@linux.com](mailto:montana@linux.com).
