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
<b>3.</b> Run all environment using docker compose (there are two instances of each app)

<b>4.</b> Applications will automatically register to eureka. 

<b>5.1.</b> RF-agents will register and authenticate to user-service. Then will register to sensor-service and start send data to gateway-service.

<b>5.2.1.</b> For register camera agents you must first create account. To do that use register endpoint: 
```
POST localhost:8003/camera/register  
{
    "userName":"name@gmail.com",
    "password":"password",
    "roles":[
        "CameraRegistrationRole"
    ]
}
```
<b>5.2.2.</b> Then you will be able to authenticate and receive token
```
POST localhost:8003/authenticate
{
    "userName":"name@gmail.com",
    "password":"password"
}
``` 
<b>5.2.3.</b> Previously generated token will be needed to register camera sensors(agents). Add Header Authorization with value "Bearer + {token}"
```
POST 
{
    "streamAddress":"112.223.222.22",
    "panTiltZoom":{"pan":222,"tilt":117,"zoom":8},
    "sensorStatus":"ACTIVE",
    "ip":"(here write your IP):(here write port of camera agent - 8005 or 8006)"
}
```
<b>6.</b> All the time, data will be generated, sent, saved in the database and sent to the appropriate kafka topic