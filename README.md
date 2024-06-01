# <img src="images/FreeCAD-symbol.svg" style="width:10%;" /> Developers Handbook

This is the repo for the [Developers Handbook](https://freecad.github.io/DevelopersHandbook/).

## Install and run locally

This handbook uses Jekyll, a static site generator written in Ruby. There are several options for setting up local development. Examples below illustrate the process using Bundler on a Debian-based system, or using a Docker container to keep things separate from the host system. After setup, the handbook will be available at `http://127.0.0.1:4000`.

### Debian-based system setup

Although Jekyll is packaged in Debian, the `github-pages` gem is not, so Bundler is required to handle dependencies. It is not necessary to `apt install jekyll`.

Bundler requires `ruby-dev` and `build-essential` to build Ruby gem native extensions.

```bash
$ sudo apt install ruby-bundler ruby-dev build-essential
$ bundle install
$ bundle exec jekyll serve
```

### Docker container setup

Docker bind mounts the handbook source directory inside the container and marks it as private & unshared with any other container, before calling `jekyll serve` as in the instructions above.

```bash
$ docker run --rm --volume="$PWD:/srv/jekyll:Z" -it jekyll/jekyll jekyll serve
```

### References

- [Jekyll installation guidelines](https://jekyllrb.com/docs/installation/)
- [Ruby 101 notes](https://jekyllrb.com/docs/ruby-101/)
