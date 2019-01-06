# install ruby dependencies
FROM ruby:2.5.1
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

# set working directory
ENV RAILS_ROOT /var/www/rails_app
RUN mkdir -p $RAILS_ROOT
WORKDIR $RAILS_ROOT

# add gem files
COPY rails_app/Gemfile       $RAILS_ROOT/Gemfile
COPY rails_app/Gemfile.lock  $RAILS_ROOT/Gemfile.lock

# install dependencies 
RUN bundle install

# add ruby on rails project
COPY rails_app $RAILS_ROOT

# set Ruby On Rails application container's port number
EXPOSE 3000

# run puma
CMD [ "bundle", "exec", "puma", "-C", "config/puma.rb" ]
