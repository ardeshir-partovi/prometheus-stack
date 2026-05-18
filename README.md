1. Make a directory, get and copy Prometheus and Grafana settings files:<br>
   `mkdir monitoring && cd monitoring && touch docker-compose.yml`<br>
   `docker run -dit --name grafana-test grafana/grafana`<br>
   `docker run -dit --name prometheus-test prom/prometheus`<br>
   `docker cp prometheus:/etc/prometheus/prometheus.yml .`<br>
   `docker cp grafana:/etc/grafana/grafana.ini .`<br>

   vi docker-compose.yml:
```
services:

  prometheus:
    image: prom/prometheus
    container_name: prometheus
    restart: unless-stopped
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
    ports:
      - 9090:9090

  pushgateway:
    image: prom/pushgateway
    container_name: pushgateway
    restart: unless-stopped
    ports: 
      - 9091:9091

  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: unless-stopped
    volumes:
      - ./grafana.ini:/etc/grafana/grafana.ini:ro
    ports:
      - 3000:3000
```

2. Deploy Prometheus, Grafana, Pushgateway using `docker compose up -d`
