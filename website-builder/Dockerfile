FROM ruby:2.7-alpine

LABEL maintainer="Debezium Community"

ENV SITE_HOME=/site

RUN apk add --no-cache build-base gcc bash cmake git jq nodejs yarn

# Install Antora framework
RUN yarn global add @antora/cli@2.3.4 @antora/site-generator-default@2.3.4 \
    && rm -rf $(yarn cache dir)/* \
    && find $(yarn global dir)/node_modules/asciidoctor.js/dist/* -maxdepth 0 -not -name node -exec rm -rf {} \; \
    && find $(yarn global dir)/node_modules/handlebars/dist/* -maxdepth 0 -not -name cjs -exec rm -rf {} \; \
    && find $(yarn global dir)/node_modules/handlebars/lib/* -maxdepth 0 -not -name index.js -exec rm -rf {} \; \
    && find $(yarn global dir)/node_modules/isomorphic-git/dist/* -maxdepth 0 -not -name for-node -exec rm -rf {} \; \
    && rm -rf $(yarn global dir)/node_modules/moment/min \
    && rm -rf $(yarn global dir)/node_modules/moment/src \
    && rm -rf /tmp/*

# Install Rake and Bundler. This is the minimum needed to generate the site ...
RUN gem install bundler -v "~>1.0" \
    && gem install rdoc -v 6.2.0 \
    && gem install bundler \
    && gem install jekyll -v ">= 3.8.3" \
    && gem install rake


#Copy over the gemfile to a temporary directory and run the install command. 
WORKDIR /tmp
ADD Gemfile Gemfile
ADD Gemfile.lock Gemfile.lock 
RUN bundle install 

WORKDIR $SITE_HOME
 
EXPOSE 4000
 
# And execute 'run' by default ...
CMD ["run"]
