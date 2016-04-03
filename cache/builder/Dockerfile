FROM envygeeks/alpine
MAINTAINER Bunto Core <hello@bunto.isc>
COPY copy/ /

ENV LANGUAGE=en_US LANG=en_US.UTF-8 LC_ALL=en_US BUNTO_ENV=production

ENV \
  BUNTO_GIT_URL=https://github.com/bunto/bunto.git \
  BUNTO_VERSION=bunto@1.0.0
RUN \
  echo "builder" > /image && \
  apk --update add zlib python libxml2 zlib-dev build-base libxslt-dev readline-dev ruby-io-console libxml2-dev libffi-dev ruby-dev yaml-dev readline libxslt ruby yaml libffi nodejs ruby-irb ruby-json ruby-rake ruby-rdoc nginx git bash openssh-client lftp && \

  

  mkdir -p /home/bunto && \
  addgroup -Sg 1000 bunto &&  \
  adduser  -SG bunto -u 1000 -h /home/bunto bunto && \
  chown bunto:bunto /home/bunto && \

  cd ~ && \
  yes | gem update --system --no-document -- --use-system-libraries && \
  yes | gem update --no-document -- --use-system-libraries && \

  repo=$(docker-helper git_clone_ruby_repo "1.0.0") && \
  if [ ! -z "$repo" ]; \
  then \
    cd $repo && \
    rake build && gem install pkg/bunto-*.gem --no-document -- \
      --use-system-libraries && \
    rm -rf $repo; \
  else \
    yes | docker-helper ruby_install_gem \
      "bunto@1.0.0" --no-document -- \
        --use-system-libraries; \
  fi && \

  cd ~ && \
  mkdir -p /usr/share/ruby && \
  
    echo "pygments.rb bunto-sitemap bunto-mentions bunto-coffeescript bunto-sass-converter bunto-redirect-from bunto-compose bunto-feed rdiscount redcarpet kramdown jemoji RedCloth maruku html-proofer" > /usr/share/ruby/default-gems && \
  

  docker-helper install_default_gems && \
  apk del zlib-dev build-base libxml2-dev libxslt-dev readline-dev libffi-dev ruby-dev yaml-dev nginx && \
  gem clean && gem install bundler --no-document && \

  mkdir -p /srv/bunto && \
  chown bunto:bunto /srv/bunto && \
  echo 'bunto ALL=NOPASSWD:ALL' >> /etc/sudoers && \
  rm -rf /usr/lib/ruby/gems/*/cache/*.gem && \
  docker-helper cleanup
WORKDIR /srv/bunto
EXPOSE 4000 80
