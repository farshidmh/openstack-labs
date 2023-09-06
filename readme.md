# Openstack Labs

![logo.png](logo.png)

Labs instructions for Openstack training sessions.

## Introduction

This repository contains the labs for the Openstack training sessions. The labs are divided into two sections:

## Labs Structure

Each lab is a markdown file that contains the following sections:

- Duration: The estimated time to complete the lab.
- Objective: The objective of the lab.
- Pre-requisites: The pre-requisites for the lab.
- Step-by-Step Guide: The step-by-step guide to complete the lab.
- Summary: The summary of the lab.

## Pre-requisites:

- Access to the OpenStack CLI
- Openstack installed on Ubuntu 22.04

## Labs

### 1. Installation

- 1.1 - [Installing Openstack using Packstack](install/packstack.md) (Centos, Educational)
- 1.2 - [Installing Openstack using Kolla-Ansible - All-in-One](install/kolla-ansible-aio.md) (Ubuntu, Educational)
- 1.3 - [Installing Openstack using Kolla-Ansible - Multi Node](install/kolla-ansible-multinode.md) (Ubuntu, Production)

### 2. Using Openstack

- 2.1 [Create Ubuntu Image using CLI](image/cli.md)
- 2.2 [Create Cirros Image using CLI](image/convert_image.md)
- 2.3 [Create Image using Horizon](image/horizon.md)
- 2.4 [Create Flavor using CLI](flavor/cli.md)
- 2.5 [Create Flavor using Horizon](flavor/horizon.md)
- 2.6 [Limit flavor access to a specific project](flavor/access.md)
- 2.7 [Create a Network using CLI](network/cli.md)
- 2.8 [Create a Router using CLI](network/router.md)
- 2.9 [Create a Volume using CLI](volume/cli.md)

### 3. Keystone

- 3.1 [Create a User in CLI](keystone/user.md)

### 4. Heat

- 4.1 [Create a Heat Stack using CLI](heat/simple_template.md)
- 4.2 [Create a Heat Stack with Network using CLI](heat/network_instance.md)
- 4.3 [Create a Heat Stack with Parameters using CLI](heat/template_parameter.md)
- 4.4 [Create a Heat Stack by reusing another template](heat/nested.md)
- 4.5 [Create a Heat Stack with advanced networking](heat/advance_networking.md)

### Extras

- [How to setup LVM on Ubuntu 22.04 for Cinder](extra/ubuntu-lvm.md)
- [How to setup Ansible on Ubuntu 22.04 for Kolla-Ansible](extra/ubuntu-ansible.md)