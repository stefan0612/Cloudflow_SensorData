# Cloudflow_SensorData


## File Tree

├── build.sbt
├── LICENSE
├── project <- Cloudflow Plugins
│   └── cloudflow-plugins.sbt
├── README.md
└── src
    └── main
        ├── avro <- Insert Avros here
        │   ├── InvalidMetric.avsc
        │   ├── Measurements.avsc
        │   ├── Metric.avsc
        │   └── SensorData.avsc
        ├── blueprint <- Insert Blueprints here
        │   └── blueprint.conf
        ├── resources <- Default Logger
        │   ├── local.conf
        │   └── log4j.xml
        └── scala <- Put Scala Code and Packages here
            ├── package.scala
            └── sensordata
                ├── InvalidMetricLogger.scala
                ├── JsonFormats.scala
                ├── MetricsValidation.scala
                ├── SensorDataHttpIngress.scala
                ├── SensorDataToMetrics.scala
                ├── SensorDataUtils.scala
                └── ValidMetricLogger.scala

## Requirements

* Java 8 or higher
* Sbt 1.2.8 or higher
* Docker 18.09 or higher (Used for local deployment)

## Verify Blueprint of Application

```
sbt verifyBlueprint
```

Verifies Streamlet connections, invalid connections, avro schemas and more

## Run App in Sandbox Mode

```
sbt runLocal
```

Application runs locally and can be requested on port 3000

## Sending Data to Cloudflow

### Valid data example:

```
curl -i -X POST localhost:3000 -H "Content-Type: application/json" --data '{"deviceId":"c75cb448-df0e-4692-8e06-0321b7703992","timestamp":1495545646279,"measurements":{"power":1.7,"rotorSpeed":3.9,"windSpeed":105.9}}'
```

### Invalid data example:

```
curl -i -X POST localhost:3000 -H "Content-Type: application/json" --data '{"deviceId":"c75cb448-df0e-4692-8e06-0321b7703992","timestamp":1495545646279,"measurements":{"power":1.7,"rotorSpeed":3.9,"windSpeed":-105.9}}'
```

Logs can be seen in **sensor-data-scala-local.log**, the path to the file file is normally printed to console after running 'sbt runLocal'. 

Most likely its in the /tmp folder