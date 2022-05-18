## nyc w/ Travis by Montana Mendy 

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
- npm install -g nyc
```

For `nyc` to install completely. Let's now connect our app to our mock Mongo server: 

```bash
sudo systemctl start mongo
```

Now that we are connected, let's run our app: 

```bash
node bin/www & 
```

Important to have `&` after `node bin/www` or your build will get stuck in Travis, this tells Travis to have it run as a background process. 

## Testing with nyc 

Now, time to test your project with `nyc`, lets run:

```bash
nyc npm run test
```
 




