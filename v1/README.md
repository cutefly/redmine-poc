# Upgrade redmine v3 -> v5

## Install redmine:3.3

```
$ docker-compose up
$ docker-compose stop

# copy plugins, themes, mysql-bakcup.sql

# restore mysql

$ docker start mysql
$ docker exec -it mysql bash
mysql# mysql -u root -p redmine < /var/lib/mysql/redmine_backup.sql

$ docker start redmine
```

## Upgrade 3.4.18

```
$ docker-compose down
change tag : redmine:3.3 -> redmine:3
$ docker-compose up

# perfome the upgrade
$ docker exec -it redmine bash
redmine# bundle install --without development test

# update the database
redmine# bundle exec rake db:migrate RAILS_ENV=production
redmine# bundle exec rake redmine:plugins:migrate RAILS_ENV=production

# clean up
redmine# bundle exec rake tmp:cache:clear RAILS_ENV=production

```
