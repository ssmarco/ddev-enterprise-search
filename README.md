[![tests](https://github.com/ddev/ddev-enterprise-search/actions/workflows/tests.yml/badge.svg)](https://github.com/ddev/ddev-enterprise-search/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-enterprise-search <!-- omit in toc -->

- [Introduction](#introduction)
- [Getting started](#getting-started)
- [How to debug in Github Actions](#how-to-debug-tests-github-actions)

## Introduction

ddev-enterprise-search is the un-official implementation of Elastic Enterprise Search service for DDEV based on their Docker guide\*.

Enterprise Search is an additional Elastic service that adds APIs and UIs to those already provided by Elasticsearch and Kibana.

Currently sitting at version 8.12.0, part of the implementation as a service for DDEV includes Kibana container.
This means that to use this service, existing Kibana service needs to be uninstalled in your project and should install the supported Elastic Search from DDEV.

From your DDEV project, install this by running `ddev get ssmarco/ddev-enterprise-search` then `ddev restart`.
This will take up to 30 minutes or so due when downloading the required docker containers (Elastic Search, Kibana and Enterprise Search).

- [Reference](https://www.elastic.co/guide/en/enterprise-search/current/start.html)
- [Docker guide\*](https://www.elastic.co/guide/en/enterprise-search/current/docker.html)

## Getting started

1. In the DDEV project directory launch the command:

```
ddev get ddev/ddev-elasticsearch
ddev get ssmarco/ddev-enterprise-search
```

2. Restart the DDEV instance:

```
ddev restart
```

3. Get the URL of the Kibana dashboard eg (https://your-project-name.ddev.site:5602):

```
ddev describe
```

4. Login with the username, `elastic` and password, `elastic`

## Configuring your framework

### Silverstripe

1. Update your project's `.env` file. The API keys are found in the Enterprise Search section of Kibana dashboard.

```
ENTERPRISE_SEARCH_ENGINE_PREFIX="my-index"
ENTERPRISE_SEARCH_API_KEY="private-xxxxxxxxxxxx-change-this"
ENTERPRISE_SEARCH_API_SEARCH_KEY="search-xxxxxxxxxxxx-change-this"
ENTERPRISE_SEARCH_ENDPOINT="http://enterprisesearch:3002"
```

2. The Enterprise Search endpoint is `http://enterprisesearch:3002`

## Troubleshooting

1. Make sure all required containers are downloaded:

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.12.0
docker pull docker.elastic.co/kibana/kibana:8.12.0
docker pull docker.elastic.co/enterprise-search/enterprise-search:8.12.0
```

2. Remove container volumes to restart from scratch.

List all existing volumes from your system:

```
docker volume ls
```

This will show example output below:

```
DRIVER    VOLUME NAME
local     ddev-your-project-name_elastic-certs
local     ddev-your-project-name_elastic-data
local     ddev-your-project-name_elastic-kibana
local     ddev-your-project-name_enterprise-data
```

Delete the volumes by running:

```
docker volume rm ddev-your-project-name_elastic-certs \
ddev-your-project-name_elastic-data \
ddev-your-project-name_elastic-kibana \
ddev-your-project-name_enterprise-data
```

3. Restart by `ddev restart`

4. Check the status of the project by `ddev status`

5. Check the logs:

```
ddev logs -s elastic-config
ddev logs -s elasticsearch
ddev logs -s kibana
ddev logs -s enterprisesearch
```

6. Check job health:

```
docker inspect --format "{{json .State.Health }}" ddev-your-project-name-enterprisesearch | jq
docker inspect --format "{{json .State.Health }}" ddev-your-project-name-kibana | jq
docker inspect --format "{{json .State.Health }}" ddev-your-project-name-elasticsearch | jq
```

## Contribute

- Anyone is welcome to submit a PR to this repo. See README.md at https://github.com/ddev/ddev-addon-template, the parent of this repo.

## Maintainer

- Contributed and maintained by [Marco Hermo](https://github.com/ssmarco).
