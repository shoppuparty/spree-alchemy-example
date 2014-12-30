# Amazon Web Services (AWS) Elastic Beanstalk

Deployable to AWS Elastic Beanstalk `64bit Amazon Linux 2014.09 v1.0.9 running Ruby 2.1 (Puma)`.

[`Gemfile`]()

```
$ bundle install
```

Create `.env` with the following key-value pairs. Do not check this file into your repository, the purpose of it is to centralize sensitive credentials and to avoid committing them. These credentials are loaded into your development environment when using `foreman` and mirror your AWS Elastic Beanstalk ENV.

`WORKERS`, `PORT`, `RACK_ENV`, `MIN_THREADS`, `MAX_THREADS` are used by Puma and values can be changed to better match your production Puma webserver.

`SECRET_KEY_BASE` is only needed if `RACK_ENV` is set to `production`. This helped to resolve some deployment issues surrounding not having a `secret` for `production`.

`AWS_ACCESS_KEY_ID` and `AWS_SECRET_KEY` are the default naming for your AWS S3 Credentials by AWS Elastic Beanstalk. AssetSync’s default naming for these same keys differ slightly so go with the AWS EB default naming.

`.env`

```
WORKERS=3
PORT=3000
RACK_ENV=development
MIN_THREADS=1
MAX_THREADS=16
SECRET_KEY_BASE=
AWS_ACCESS_KEY_ID=
AWS_SECRET_KEY=
FOG_PROVIDER=AWS
FOG_DIRECTORY=
FOG_REGION=
```

Update your `.gitignore` to prevent committing your `.env`
[`.gitignore`]()

Create `Procfile` for `foreman` to load Puma configuration
[`Procfile`]()

Create Puma configuration
[`config/puma.rb`]()

Create `asset_sync` initializer
[`config/initializers/asset_sync.rb`]()




# Included Feature Branches

- [`feature/postgresql`](https://github.com/shoppuparty/spree-alchemy-example/tree/feature/postgresql)

