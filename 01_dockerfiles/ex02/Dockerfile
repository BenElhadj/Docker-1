# Pour le tester:
# docker build -t ft-rails:on-build .
# docker inspect ft-rails:on-build (pour verifier les tags onbuild)
# git clone https://github.com/mjhea0/rails-helloworld app 
# (supprimer le fichier Gemfile.lock dans app)
# Creer un fichier Dockerfile-fils comme dans le sujet
# docker build -t testruby -f Dockerfile-fils .
# docker run -p 3000:3000 -d testruby
# Ouvrir 192.168.99.100:3000




FROM debian:latest
LABEL maintainer="bhamdi <bhamdi@student.42.fr>"

RUN apt-get update
RUN apt-get install -y --no-install-recommends nodejs postgresql sqlite3 build-essential postgresql-contrib postgresql-server-dev-all mysql-server ruby-full libsqlite3-dev
RUN gem install bundler rails-server activerecord-postgresql-adapter postgresql

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle install && rake db:migrate && rake db:seed
