SHELL := /bin/bash
.POSIX:
.PHONY: help env install upgrade-hugo serve build start initial

help: ## show help
	@egrep -h '\s##\s' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

env:
	pip3 install pipenv

shell: ## enter the virtual environment
	pipenv shell

install: ## install or update dependencies
	pipenv install --dev
	# npm install
	npm install -g markdownlint-cli
	pre-commit install --install-hooks

HUGO_VERSION:=$(shell curl -s https://api.github.com/repos/gohugoio/hugo/releases/latest | grep 'tag_name' | cut -d '"' -f 4 | cut -c 2-)

upgrade-hugo: ## update hugo
	mkdir tmp/ && \
	cd tmp/ && \
	curl -sSL https://github.com/gohugoio/hugo/releases/download/v$(HUGO_VERSION)/hugo_extended_$(HUGO_VERSION)_Linux-64bit.tar.gz | tar -xvzf- && \
	sudo mv hugo /usr/local/bin/ && \
	cd .. && \
	rm -rf tmp/
	hugo version

serve:  ## run the local development server
	## --enableGitInfo: add git information to pages
	hugo serve --enableGitInfo --disableFastRender --environment development --themesDir ../themes/

future: ## run the local development server in the future
	## --enableGitInfo: add git information to pages
	## --buildFuture
	## three types of content are supported by Hugo:
	## -drafts
	## -future published 
	## -expired
	## --disableFastRender: full render on changes
	hugo serve --enableGitInfo --buildFuture --disableFastRender --environment development --themesDir ../themes/ 

draft:
	hugo serve --enableGitInfo --buildDrafts --disableFastRender --environment development --themesDir ../themes/ 

build: ## lock dependencies and build site
	pipenv lock
	hugo --minify --themesDir ../themes/ --destination ../docs/

start: upgrade-hugo serve ## update Hugo and start development server

initial: env install upgrade-hugo serve ## install tools and start development server
