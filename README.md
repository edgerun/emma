EMMA
====

EMMA is an **E**lastic **M**QTT **M**iddlew**A**re.

Description
-----------

EMMA is an effort to create an elastic topic-based publish-subscribe system
that works natively in edge computing scenarios.
Its core components are:

* **Gateways** to transparently connect MQTT clients to the system
* **Sidecars** that wrap MQTT protocol servers with a thin overlay network and control plane 
* The **Master** to orchestrate the system
* A **Control & Monitoring Protocol** implemented by all components that acts as the orchestration medium

The following figure illustrates the architecture:

![EMMA Architecture](http://dsg.tuwien.ac.at/team/trausch/projects/images/emma-architecture.svg)

Running EMMA
------------

### Prerequisites

We use [Redis](https://redis.io) and its [keyspace notification](https://redis.io/topics/notifications)
mechanism to distribute subscription tables.
An EMMA deployment requires a running Redis instance with keyspace notifications activated.
To activate them for a running Redis instance use the `redis-cli` command

    redis-cli config set notify-keyspace-events KEA

### Starting the controller

The controller is a [Spring Boot](https://projects.spring.io/spring-boot)
application and can be run with

    java -jar emma-controller/target/emma-controller-<version>.jar

The controller requires four available ports (which can be parameterized via `application.yml`,
but the first two are also encoded in the properties files of the broker and gateway components):
* `60042` for the broker heartbeats
* `60043` for the monitoring protocol
* `50042` for the controller shell

The Spring Boot application starts a web server on port `8080`.
The controller also starts an [Orvell](https://git.dsg.tuwien.ac.at/trausch/orvell)
shell that you can connect to via TCP with `nc` or `telnet` by running

    nc localhost 50042

### Starting a broker instance

**TODO**: we are re-designing the broker/overlay architecture and will update 

### Starting a gateway instance

**TODO**: we are currently re-implementing the client gateways in go


### Connecting to a gateway

You can use any MQTT compliant client to connect to the system.
For example, you can use the [Mosquitto](https://mosquitto.org) cli clients to
connect to a running gateway 

    mosquitto_pub -d -i console-01 -t '<topic>' -p <port> -l

and

    mosquitto_sub -d -i console-02 -t '<topic>' -p <port>


Related publications
--------------------


1. Rausch, T., Dustdar, S., & Ranjan, R. (2018).
   Osmotic Message-Oriented Middleware for the Internet of Things.
   *IEEE Cloud Computing*, 5(2), 17--25.
   [[Preprint](http://www.infosys.tuwien.ac.at/Staff/sd/papers/Zeitschriftenartikel_2018_S_Dustdar_Osmotic.pdf)]
1. Rausch, T., Nastic, S., & Dustdar, S. (2018).
   EMMA: Distributed QoS-Aware MQTT Middleware for Edge Computing Applications. 
   In *2018 IEEE International Conference on Cloud Engineering (IC2E)*.
   [[Preprint](http://dsg.tuwien.ac.at/staff/trausch/pub/PID5190461.pdf)]
   [[Presentation](https://www.slideshare.net/ThomasRausch4/emma-distributed-qosaware-mqtt-middleware-for-edge-computing-applications)]
1. Rausch, T. (2017).
   Message-oriented Middleware for Edge Computing Applications.
   In *Proceedings of the 18th Doctoral Symposium of the 18th International Middleware Conference* (pp. 3--4).
   [[Preprint](http://dsg.tuwien.ac.at/staff/trausch/pub/middleware17ds-paper2.pdf)]
   [[Presentation](https://www.slideshare.net/ThomasRausch4/messageoriented-middleware-for-edge-computing-applications)]
