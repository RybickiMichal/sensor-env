# Sensor environment
<b>1.</b> Before build docker images change application.properties for:
- rf-service: 
```
kafka.bootstrap-servers={here write your IP}:9092
```
- camera-service: 
```
kafka.bootstrap-servers={here write your IP}:9092
```
- rf-agent: 
```
gateway.url=http://{here write your IP}:8003/
```
<b>2.</b> Execute for every project:
```
"mvn spring-boot:build-image"
```
<b>3.</b> Run all environment (two instances of each app) using docker compose

<b>4.</b> Applications will automatically register to eureka. 

<b>5.1.</b> RF-Agent will register(if not already registered yet) and authenticate to user-service. Nextly will register to sensor-service and start send data to rf-service.

<b>5.2.</b> To register camera-agent you must do it yourself. You should Register user and authenticate. With token you will be able to register camera-agent. Remember about Authorization header: 
```
"Authorization" "Bearer (token_value)".
``` 
To register use endpoint POST 
```
localhost:8003/camera/register  
```
and appropriate body. For example:
```
{
    "streamAddress":"112.223.222.22",
    "panTiltZoom":{"pan":222,"tilt":117,"zoom":8},
    "sensorStatus":"ACTIVE",
    "ip":"(here write your IP):(here write port of camera agent - 8005 or 8006)"
}
```
<b>6.</b> all the time, data will be generated, sent, saved in the database and sent to the appropriate kafka topic