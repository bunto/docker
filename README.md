# Bunto Docker

[![Build Status](https://travis-ci.org/bunto/docker.svg?branch=master)][1]

Bunto Docker is a software image that has Bunto and many of it's dependencies ready to use for you in an encapsulated format.  It includes a default set of gems, different image types with different extra packages, and wrappers to make Bunto run more smoothly from start to finish for most Bunto users. If you would like to know more about Docker you can visit https://docker.com and if you would like to know more about Bunto you can visit https://github.com/bunto/bunto

## Image Types

* `bunto/bunto`: Default image.
* `bunto/minimal`: Very minimal image.
* `bunto/builder`: Includes tools.

### Standard

The standard images (`bunto/bunto`) include a default set of "dev" packages, along with Node.js, and other stuff that makes Bunto easy.  It also includes a bunch of default gems that the community wishes us to maintain on the image.

#### Usage

```sh
export BUNTO_VERSION=3.4
docker run --rm \
  --volume=$PWD:/srv/bunto \
  -it bunto/bunto:$BUNTO_VERSION \
  bunto build
```

### Builder

The builer image comes with extra stuff that is not included in the standard image, like `lftp`, `openssh` and other extra packages meant to be used by people who are deploying their Bunto builds to another server with a CI.

#### Usage

```sh
export BUNTO_VERSION=3.4
docker run --rm \
  --volume=$PWD:/srv/bunto \
  -it bunto/builder:$BUNTO_VERSION \
  bunto build
```

### Minimal

The minimal image skips all the extra gems, all the extra dev dependencies and leaves a very small image to download.  This is intended for people who do not need anything extra but Bunto.

#### Usage

***You will need to provide a `.apk` file if you intend to use anything like Nokogiri or otherwise, we do not install any development headers or dependencies so C based gems will fail to install.***

```sh
export BUNTO_VERSION=3.4
docker run --rm \
  --volume=$PWD:/srv/bunto \
  -it bunto/minimal:$BUNTO_VERSION \
  bunto build
```

## Config

You can configure some pieces of Bunto using environment variables, what you cannot with environment variables you can configure using the Bunto CLI.  Even with a wrapper, we pass all arguments onto Bunto when we finally call it.

* `FORCE_POLLING`: `true`, `false`, `""`
* `BUNTO_DEBUG`, `VERBOSE`: `true`, `false`, `""`
* `BUNDLE_CACHE`:`true`, `false`, `""`

If you would like to know the CLI options for Bunto, you can visit [Bunto's Help Site][2]

## Packages

You can install system packages by providing a file named `.apk` with one package per line.  If you need to find out what the package names are for a given command you wish to use you can visit https://pkgs.alpinelinux.org. ***We provide many dependencies for most Ruby stuff by default for `builder` and standard images.  This includes `ruby-dev`, `xml`, `xslt`, `git` and other stuff that most Ruby packages might need.***

### Tools

You will find directions for using our image with various tools.

### Docker-Compose
```yml
services:
  site:
    command: bunto serve
    image: bunto/bunto:latest
    volumes:
      - $PWD:/srv/bunto
    ports:
      - 4000:4000
      - 35729:35729
      - 3000:3000
      -   80:4000
```

#### Usage

```sh
docker-compose run site bunto new site
docker-compose run site --service-ports bunto s
docker-compose run site bunto b
```

### LiveReload

This image supports LiveReload, all you need do is add LiveReload to your Bunto Pluins, and then map the port, and your browser should be able to communicate with your LiveReload listener.

```rb
group :plugins do
  gem "bunto-livereload"
end
```

#### Usage

```sh
export BUNTO_VERSION=3.4
docker run --rm \
  --volume=$PWD:/srv/bunto \
  -p 35729:35729 -p 4000:4000 \
  -it bunto/builder:$BUNTO_VERSION \
  bunto build
```

## Building Our Images

You can build our images or any specific tag of an image with `bundle exec docker-template build` or `bundle exec docker-template build repo:tag`, yes it's that simple to build our images; even if it looks complicated it's not.

## Contributing

* Fork the current repo; `bundle install`
* `opts.yml` holds most of the versions, and gems.
* Test your image manually `script/debug` will help you with that.
* Ensure that your intended changes work as they're supposed to.
* Ship a pull request if you wish to have it reviewed!

[1]: https://travis-ci.org/bunto/docker
[2]: https://buntowaf.tk/docs/configuration/#build-command-options
