# Amazon Web Services (AWS) Elastic Beanstalk

Deployable to AWS Elastic Beanstalk `64bit Amazon Linux 2014.09 v1.0.9 running Ruby 2.1 (Puma)` with Postgres (RDS).

[`Gemfile`]()

```
$ bundle install
```

Create `.env` with the following key-value pairs. Do not check this file into your repository, the purpose of it is to centralize sensitive credentials and to avoid committing them. These credentials are loaded into your development environment when using `foreman` and mirror your AWS Elastic Beanstalk ENV.

`WORKERS`, `PORT`, `RACK_ENV`, `MIN_THREADS`, `MAX_THREADS` are used by Puma and values can be changed to better match your production Puma webserver.

`SECRET_KEY_BASE` is only needed if `RACK_ENV` is set to `production`. This helped to resolve some deployment issues surrounding not having a `secret` for `production`.

`AWS_ACCESS_KEY_ID` and `AWS_SECRET_KEY` are the default naming for your AWS S3 Credentials by AWS Elastic Beanstalk. AssetSync’s default naming for these same keys differ slightly so go with the AWS EB default naming.

`FOG_PROVIDER`, `FOG_DIRECTORY` and `FOG_REGION` are used by AssetSync to upload your assets. `FOG_DIRECTORY` is the AWS S3 Bucket that serves as the parent namespace.

`.env`

```
PORT=3000
RACK_ENV=development
SECRET_KEY_BASE=
AWS_ACCESS_KEY_ID=
AWS_SECRET_KEY=
FOG_PROVIDER=AWS
FOG_DIRECTORY=
FOG_REGION=
```

Update your `.gitignore` to prevent committing your `.env` and manifest files created by asset compilation.
[`.gitignore`]()

Create `Procfile` for `foreman` to load Puma configuration.
[`Procfile`]()

Create Puma configuration.
[`config/puma.rb`]()

Create `asset_sync` initializer.
[`config/initializers/asset_sync.rb`]()

Create `state_machine` initalizer to ignore `Instance method "open" is already defined in Object, use generic helper instead or set StateMachine::Machine.ignore_method_conflicts = true.` warning.
[`config/initializers/state_machine.rb`]()

Update `production` environment settings. This is important because it namespaces your assets based on the environment which gets carried over into your AWS S3 Bucket.
[`config/environments/production.rb`]()

```
$ eb init

# set eb env to match (as needed)
$ eb setenv AWS_ACCESS_KEY_ID=[value] AWS_SECRET_KEY=[value] FOG_DIRECTORY=[value] FOG_REGION=[value]
```
Create EB config file for options that are not environment dependent (within Elastic Beanstalk)
[`.ebextensions/01_option_settings.config`]()

Create EB config file for server packages necessary for deployment
[`.ebextensions/02_packages.config`]()


# Local Development

Whenever credentials are needed, instead of running `rails s` in development now `foreman s` can be run which will load the `Procfile` and use Puma webserver as well as load the `.env` file with credentials.


# Deployment

Having AWS Elastic Beanstalk perform `rake assets:precompile` makes for very slow deployments. By using AssetSync and Foreman, this task can be performed locally. 

- Verify that AWS EB Environment Properties for your application is set to `RAILS_SKIP_ASSET_COMPILATION=true`.
- `.gitignore` is ignoring all `public/assets` except `manifest*.json` files.
- `.env` AWS/FOG credentials are correct with necessary permissions for `asset_sync` to upload properly.

```
# compile assets for `production` locally and upload to AWS S3, foreman is required for loading credentials
$ foreman run rake assets:precompile RAILS_ENV=production

# add and commit changes
$ git add [file(s)]
$ git commit -m "[message]"

# deploy to EB
$ eb deploy
```


# Included Feature Branches

- [`feature/postgresql`](https://github.com/shoppuparty/spree-alchemy-example/tree/feature/postgresql)

