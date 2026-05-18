<b>You Must install docker first.<br>
Commands run as root.</b>


1. Make a directory, get and copy Prometheus and Grafana settings files:<br>
   ```
   mkdir ~/monitoring && \
   cd ~/monitoring && \
   touch docker-compose.yml && \
   docker run -dit --name prometheus-test prom/prometheus && \
   docker run -dit --name grafana-test grafana/grafana && \
   docker cp prometheus-test:/etc/prometheus/prometheus.yml . && \
   docker cp grafana-test:/etc/grafana/grafana.ini . && \
   docker rm -f prometheus-test grafana-test
   ```

2. Use any editor vi/nano to copy/paste the below yaml content to `~/monitoring/docker-compose.yml` file:
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

3. Run `docker compose up -d`
