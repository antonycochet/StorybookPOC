# Demo

* Create network: `docker network create nettest`
* Run traefik container: 
  ```
    docker run --rm -d -p 80:80 --name traefik --network nettest \
      -l 'traefik.enable=true' \
      -l 'traefik.http.services.traefik-api.loadbalancer.server.port=8080' \
      -l 'traefik.http.routers.dashboard.rule=Host(`traefik.localhost`)' \
      -l 'traefik.http.routers.dashboard.service=api@internal' \
      -v /var/run/docker.sock:/var/run/docker.sock \
      traefik:2.3 --api=true --api.dashboard=true  --providers.docker=true
  ```
* http://traefik.localhost/
* Run mock-api: 
  ```
    docker run --rm -d --network nettest \
      -l 'traefik.enable=true' \
      -l 'traefik.http.services.mock-api.loadbalancer.server.port=8050' \
      -l 'traefik.http.routers.mock-api.rule=Host(`mock.localhost`)' \
      tuxivinc/mock-api:1.8
  ```
* Run 2x de plus commande ci-dessus
* http://mock.localhost/mock-api/swagger-ui/index.html?configUrl=/mock-api/v3/api-docs/swagger-config#/Info/getHostname -> Call 3x
* alternative: curl -X GET http://mock.localhost/mock-api/sniffer/headers
# StorybookPOC
