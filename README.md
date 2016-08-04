# Chadev Prometheus Demo


## Setup

```
docker-compose up  -d
# If grafana fails to come up because postgres is still initializing
docker-compose start grafana
```

## Open these links

```
open http://$(docker-machine ip):3000/
open http://$(docker-machine ip):9093/
open http://$(docker-machine ip):9090/
open http://$(docker-machine ip):8080/
```

## Grafana setup

The login to grafana is:

```
username: admin
password: SuperSecret
```

Import the `dashboard.json` into Grafana as a dashboard after adding Prometheus
as a data source. The address to give Grafana for Prometheus is
`http://prometheus:9090`.

## License
MIT License (see the [LICENSE](LICENSE))
