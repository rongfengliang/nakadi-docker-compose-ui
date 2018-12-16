# nakadi && ui docker-compose running

## how to running

* start

```code
docker-compose up -d
```

* ui

open http://localhost:3000

* api

open http://localhost:8080

* create event type

```code
curl -v -XPOST http://localhost:8080/event-types -H "Content-type: application/json" -d '{
  "name": "order.ORDER_RECEIVED",
  "owning_application": "order-service",
  "category": "undefined",
  "partition_strategy": "random",
  "schema": {
    "type": "json_schema",
    "schema": "{ \"properties\": { \"order_number\": { \"type\": \"string\" } } }"
  }
}'
```

* send  event message

> with low level api best practice is use subscription

```code
curl -v -XPOST http://localhost:8080/event-types/order.ORDER_RECEIVED/events -H "Content-type: application/json" -d '[
  {
    "order_number": "24873243241",
    "metadata": {
      "eid": "d765de34-09c0-4bbb-8b1e-7160a33a0791",
      "occurred_at": "2016-03-15T23:47:15+01:00"
    }
  }, {
    "order_number": "24873243242",
    "metadata": {
      "eid": "a7671c51-49d1-48e6-bb03-b50dcf14f3d3",
      "occurred_at": "2016-03-15T23:47:16+01:00"
    }
  }]'
```

* recevied message

```code
curl -v http://localhost:8080/event-types/order.ORDER_RECEIVED/events 
```
