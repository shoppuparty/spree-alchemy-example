# SQLite to PostgreSQL

Switch from default Rails `sqlite3` database adapter to `pg` for all environments. `production` environment is setup for Amazon Elastic Beanstalk with Amazon Relational Database Service (RDS).

[`config/database.yml`](https://github.com/shoppuparty/spree-alchemy-example/blob/feature/postgresql/config/database.yml#L1-L23)

[`Gemfile`](https://github.com/shoppuparty/spree-alchemy-example/blob/feature/postgresql/Gemfile#L7-L8)

```

# install pg adapter
$ bundle install

# setup postgresql databases and spree
$ rake db:setup

# setup alchemy with new database
$ rake alchemy:db:seed

```