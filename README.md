# KONG - Getting Started

Example of getting started stack for upgrade an stack with Kong.

## Starting kong

```bash
docker-compose up -d db
docker-compose run kong kong migrations up
docker-compose up -d kong
# docker-compose exec kong kong stop
# docker-compose exec kong kong reload
```

## Create a service

```bash
curl -v -X POST --url http://localhost:8001/services/ --data 'name=example-service' --data 'url=http://mockbin.org'
```

## Add a route for the service

```bash
curl -v -X POST --url http://localhost:8001/services/example-service/routes --data 'hosts[]=example.com'
```

## Forward your request throgh Kong

```bash
curl -v -X GET --url http://localhost:8000 --header 'Host: example.com'
```

## Enable `key-auth` plugin for the service

```bash
curl -v -X POST --url http://localhost:8001/services/example-service/plugins --data 'name=key-auth'

# Verify that plugin is properly configured. Return 401 status code
curl -v -X GET --url http://localhost:8000 --header 'Host: example.com'
```

## Create a consumer through the RESTful API

```bash
curl -v -X POST --url http://localhost:8001/consumers --data 'username=jesusgoku'

# Provision key credentials for yout consumer
curl -v -x POST --url http://localhost:8001/consumers/jesusgoku/key-auth --data 'key=jesusgoku'

# Verify that your consumer credentials are valid
curl -v -X GET --url http://localhost:8000 --header 'Host: example.com' --header 'apikey: jesusgoku'
```

## For more info

- https://docs.konghq.com/0.14.x/getting-started/quickstart
