# SQLite to PostgreSQL

Switch from default Rails `sqlite3` database adapter to `pg` for all environments. `production` environment is setup for Amazon Elastic Beanstalk with Amazon Relational Database Service (RDS).

`config/database.yml`

`Gemfile`


```
# install pg adapter
$ bundle install

# setup postgresql databases and spree
$ rake db:setup

# setup alchemy with new database
$ rake alchemy:db:seed

$ rails s
```