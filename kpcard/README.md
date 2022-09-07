# Upgrade redmine v3 -> v5

## Install redmine:3.3

```
$ docker-compose up
$ docker-compose stop

# copy files, themes, mysql-bakcup.sql
# plugins은 최종적으로 재설치

# restore mysql
$ docker start kpcard-redmine-db
$ docker exec -it -u mysql kpcard-redmine-db bash
mysql# mysql -u redmine -p redmine < /var/lib/mysql/redmine_backup.sql

$ docker start kpcard-redmine
```

## Upgrade v4[, v5]

```
$ docker-compose down
change tag : redmine:3.3 -> redmine:4.1 [ -> redmine:5 ]
$ docker-compose up

# perfome the upgrade
$ docker exec -it -u redmine kpcard-redmine bash
redmine# bundle install --without development test

# update the database
redmine# bundle exec rake db:migrate RAILS_ENV=production
redmine# bundle exec rake redmine:plugins:migrate RAILS_ENV=production

# clean up
redmine# bundle exec rake tmp:cache:clear RAILS_ENV=production

$ docker-compose restart
```

## plugin 설치

```
https://github.com/a-ono/redmine_ckeditor
https://github.com/taqueci/redmine_wysiwyg_editor

$ docker exec -it -u root kpcard-redmine bash
root# apt-get update && apt-get install -y shared-mime-info

$ docker exec -it -u redmine kpcard-redmine bash
redmine# bundle install --without development test
redmine# rake redmine:plugins:migrate RAILS_ENV=production

$ docker-compose restart
```
