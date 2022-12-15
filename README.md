# Prometheus Grafana

## Requirements

- [ ] [Docker](https://docs.docker.com/engine/install/)
- [ ] [Docker compose](https://docs.docker.com/compose/install/)
- [ ] [Aws cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) (for your existing project that will use prometheus)

## Getting started

After install the requirements and clone this repository:
```
cd existing_repo
cd prometeus
```
We're gonna configure the prometheus.yml file, replacing 'IP's variables to our IP's in order to connect with your server and the containers) application(s)
If you need yo add a new application, you can make it just adding a new job_name and configuring like the others.
```
- job_name: EC2 Docker daemon
  static_configs:
  - targets: ['IP:9323']
- job_name: application container 
  static_configs:
  - targets: ['IP:8081']
```
## Collect Docker metrics with Prometheus
There is a way that you can get your docker's metrics from the daemon to show in prometheus interface, to make it execute:
```
mv ~/prometheus-grafana/docker-daemon.json /etc/docker
```
[See more](https://docs.docker.com/config/daemon/prometheus/)

-Now you can access to this data through the port 9323 
example: ```http://IP:9323/metrics```

## Testing Prometheus
At this point we could run our containers and execute queries with prometheus to get information but the purpose is use grafana as a client to get a better experience and useful tool. If you wanna try and make sure prometheus is working well:

```
cd ~/prometheus-grafana
docker-compose up -d
docker ps
```
It will show you the containers running, there should be at least:
- prom/prometheus:latest -> port 9090:9090
- prom/node-exporter:latest -> 9100

At this time if everything is ok, you can access through port 9090 in order to get server's information
example: ```http://IP:9090/metrics```

## Access Prometheus
Access to prometheus interface located at ```http://IP:9090```
 - Verify if targets are reachables moving to: Status -> Targets
 - Check if services are "UP"(showed as green flag)
 - Try executing a simple query (go back to home page and inside expression input text)
 example query: ```engine_daemon_container_states_containers{state="running"}```

## Configuring Grafana
By default grafana use sqlite to store configurations and other things. If you wish store that into a real database and persist you need to configurate:

```
cd ~/prometheus-grafana/grafana
nano grafana.ini 
```
Into grafana.ini file search for [database] like the line 77
```
# Either "mysql", "postgres" or "sqlite3", it's your choice
type = sqlite3
host = 127.0.0.1:3306
name = grafana
user = root
# If the password contains # or ; you have to wrap it with triple quotes. Ex """#password;"""
password =
# Use either URL or the previous fields to configure the database
# Example: mysql://user:secret@host:port/database
url =
```
Save and run docker compose

- Now open url on port: ```3000```

For the first time access using admin:admin credentials and change for a better password.
Check this link to learn about grafana, how to create a dashboard? how to use? etc... 
```https://grafana.com/docs/grafana/latest/getting-started/getting-started/```

So, thats it! Have fun! 
## Test and Deploy

```
cd ~/prometheus-grafana
docker-compose up -d
```

***



## Authors
- [@grijalvamenita](https://gitlab.com/grijalvamenita)


