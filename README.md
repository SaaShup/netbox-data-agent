# Netbox Docker Agent

[![Github Issues](http://img.shields.io/github/issues/SaaShup/netbox-data-agent)](https://github.com/SaaShup/netbox-data-agent/issues)
[![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](http://opensource.org/licenses/MIT)

## Description

Agent to install on the docker server to manage containers though [netbox plugin](https://github.com/SaaShup/netbox-docker-plugin).
![netbox-data-agent](https://github.com/SaaShup/netbox-data-agent/assets/17571692/06f81159-1830-45d2-9cd0-b4a949ab086e)


## Settings

go to the [nodered admin page](http://localhost:1880/nodered) to change the settings.

## Clean
```
docker stop netbox-data-agent
docker rm netbox-data-agent
docker image rm saashup/netbox-data-agent
docker volume rm netbox-data-agent
```
## Build
```
docker build -t saashup/netbox-data-agent .
```
## Run
```
docker run -d -p 1880:1880 -v /var/run/docker.sock:/var/run/docker.sock:rw -v netbox-data-agent:/data --name netbox-data-agent -e NETBOX_TOKEN=TOKEN -e NETBOX_URL=URL saashup/netbox-data-agent
```
container must have **rw access to the docker unix socket** (/var/run/docker.sock)

Default access is *admin/saashup*

## Contribute

### Run locally

On the root of the project run:

```
npm install
DATAPATH=. npx node-red -u . -s settings_dev.js
```

Then you can browse http://localhost:1880/nodered. Default access is *admin/saashup*.

## Connect

log into [ui page](http://localhost:1880) to see your docker assets

![Screenshot from 2024-01-30 18-40-14](https://github.com/SaaShup/netbox-data-agent/assets/17571692/2437410b-734d-4601-bbd1-745041e08529)

## Monitoring
The application has a '/metrics' endpoint which can be used with prometheus to monitor if the access on the docker daemon socket is working and if all the containers are up and running.

The following metrics are currently exposed:

```
```

Example of prometheus configuration:

```
  - job_name: 'netbox-data-agent'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['IP_ADDRESS:1880']
    metrics_path: "/metrics"
    basic_auth:
      username: 'admin'
      password: 'saashup'
```

## Logs
Currently all logs are send to stdout, if netbox send an error message to the agent.

The format of the error message is the following one:
```
${data.name} level=${level[lvl]} version=${data.version} msg=${JSON.stringify(msg.msg)}
```

This can be changed by updating inside the INIT flow the "settings.js template" and change the logging.console.handler .

# Hosting
Check https://saashup.com for more information
