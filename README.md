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
If it's successful you should see this in Travis: 

<img width="1208" alt="Screen Shot 2022-05-17 at 5 11 51 PM" src="https://user-images.githubusercontent.com/20936398/168932012-2e06635f-4448-4d96-bfbb-ce504949f53f.png">

If you run it locally, you can get a GUI version of what you're seeing in the Travis build, which looks like this: 

<img width="1774" alt="Screen Shot 2022-05-17 at 5 12 42 PM" src="https://user-images.githubusercontent.com/20936398/168932096-a1426ea2-68f9-47b0-b5f9-e100168c8064.png">


## Conclusion 

This is the simplest way to use `nyc` and Travis together. This was made by myself (Montana Mendy) and as per usual if you have any questions please email me at [montana@linux.com](mailto:montana@linux.com).
