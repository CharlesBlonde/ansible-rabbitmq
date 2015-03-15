# Simple RabbitMQ Ansible PlayBook for RHEL 6.X


## Introduction

The goal of this playbook is to install RabbitMQ on a cluster of 3 (or more) instances
* Install RabbitMQ
* Configure RabbitMQ In Cluster
* Create a policy in order to have HA Queues (each queue is replicated on each server)
* Configure Virtual Hosts with one admin user on each

## Prerequisites

* [EPEL](https://fedoraproject.org/wiki/EPEL) repository must be installed. RabbitMQ RPM is available on EPEL repository

## Configuration

### host files

Must contains the list of servers on which you want to install RabbitMQ.
On one server, you must add the parameter ```rabbitmq_master=true```. All configuration (HA, vhosts, users) will be run on this server.

### hostname_master

Must be renamed to the hostname of the master on which you want to run the configuration.
Contains:
* List of virtual host to create
* User/password for each vhost

### Erlang Cookie

In order to work, an Erlang Cookie must be set with the same value on each member. It can be any random string and it is a kind of password for each member to communicate (I guess).
The value of this cookie is set inside the rabbitmq.yaml file.

Alternatively, you can override this value on the command like ```--extra-vars "erlang_cookie=RANDOM_COOKIE_STRING"``

## Run

```ansible-playbook -i host [--extra-vars "erlang_cookie=RANDOM_COOKIE_STRING"] rabbitmq.yaml```

## Limitations

The syntax of the rabbitMQ client (rabbitmqctl) has changed a few between versions and the the module used in order to create the High Availability policy is not working on old RabbitMQ versions.

If you are using and Old RabbitMQ version, you should comment the following line in the rabbitmq_policy.py module

```args.append('--priority')```