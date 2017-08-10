FROM envygeeks/alpine
LABEL maintainer "Suriyaa Kudo <isc.suriyaa@gmail.com>"
LABEL bunto.version "<%= @meta.release?? @meta.release : @meta.tag %>"
LABEL bunto.tag "<%= @meta.tag %>"

ENV LANGUAGE=en_US
ENV LANG=en_US.UTF-8
ENV BUNTO_VERSION=<%= @meta.release?? @meta.release : @meta.tag %>
ENV BUNTO_ENV=development
ENV TZ=Europe/Berlin
ENV LC_ALL=en_US
COPY copy /

<% if @meta.env? %>
  ENV <%=
    @meta.env
  %>
<% end %>

RUN apk update
RUN apk add zlib-dev build-base libxml2-dev \
  libxslt-dev readline-dev libffi-dev ruby-dev \
  yaml-dev zlib-dev libffi-dev less

RUN apk add zlib libxml2 ruby-io-console \
  readline libxslt ruby yaml libffi ruby-irb \
  ruby-json ruby-rake ruby-rdoc git openssl \
  nodejs tzdata

<% if @meta.packages? %>
  RUN apk --update add <%= @meta.packages %>
<% end %>

RUN yes | gem install --force --no-ri --no-rdoc bunto -v<%= @meta.release?? @meta.release : @meta.tag %>
RUN addgroup -Sg 1000 bunto
RUN adduser  -Su 1000 -G \
  bunto bunto

<% if @meta.gems? %>
  RUN gem install --no-ri --no-rdoc <%=
    @meta.gems
  %>
<% end %>

RUN mkdir -p /usr/share/ruby
RUN echo "<%= @meta.gems %>" > /usr/share/ruby/default-gems
RUN gem install bundler --no-ri --no-rdoc
RUN gem clean

<% if @meta.name == "minimal" %>
  RUN apk del zlib-dev build-base libxml2-dev libxslt-dev readline-dev \
    libffi-dev ruby-dev yaml-dev zlib-dev libffi-dev git
<% end %>

RUN mkdir -p /srv/bunto && chown -R bunto:bunto /srv/bunto
RUN echo 'bunto ALL=NOPASSWD:ALL' >> /etc/sudoers
RUN cleanup

WORKDIR /srv/bunto
CMD ["bunto", "--help"]
VOLUME  /srv/bunto
EXPOSE 35729 4000
