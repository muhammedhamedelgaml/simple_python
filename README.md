# Weather App - Jenkins Pipeline Deployment

This project demonstrates how to **build** and **deploy** a weather application on target machines using a **Jenkins pipeline**. The process includes containerizing the app with Docker, using Ansible to automate the installation of Docker on the target machine, pulling the Docker image from Docker Hub, and running the container. Additionally, the pipeline is integrated with **Slack** to notify about the build status (pass/fail).

## Technologies Used

- **Jenkins**: For creating and managing the continuous integration and continuous deployment (CI/CD) pipeline.
- **Docker**: To containerize the Weather App.
- **Ansible**: To automate the deployment on target machines.
- **Slack**: For sending notifications about the build status.

## Project Overview

The project automates the deployment of a weather application using Jenkins and integrates the following:

1. **Dockerization**: The app is containerized into a Docker image.
2. **Docker Hub**: The Docker image is pushed to Docker Hub for easy retrieval.
3. **Ansible Playbook**: An Ansible playbook is used to install Docker on the target machine and pull the container image from Docker Hub.
4. **Slack Notification**: The pipeline is integrated with Slack to send messages based on build success or failure.

## Features

- **Dockerized Weather App**: The app is packaged into a Docker container for portability.
- **CI/CD Pipeline**: Automated build and deployment using Jenkins.
- **Automated Deployment with Ansible**: Ansible playbooks manage the deployment process on the target machine.
- **Slack Notifications**: Integration with Slack to notify about pipeline status.

## Prerequisites

Before running the pipeline, make sure you have the following tools and services:

- **Jenkins**: For managing the pipeline.
- **Docker**: For containerizing the app and running containers.
- **Ansible**: For automating the setup and deployment on target machines.
- **Slack**: To send notifications about the build status.

## Setup Instructions

### 1. Dockerize the Weather App

The first step is to Dockerize your weather app. You can use the following Dockerfile to create the image.

```dockerfile
FROM python:3.9-slim 
WORKDIR /app 
COPY . /app/
RUN pip install -r /app/requirements.txt
EXPOSE 5000
CMD [ "python" , "/app/app.py" ]
```

### 2. write ansible-playbook install Docker engine on target machine 

install Docker engine on target machine 
```ansible-playbook
---
- name: install Docker 
  hosts: vms 
  become: yes 
  gather_facts: no 
  tasks: 
    - name: Update apt package index
      apt:
        update_cache: yes

    - name: Install required dependencies
      apt:
        name:
          - ca-certificates
          - curl
        state: present

    - name: Create keyrings directory
      file:
        path: /etc/apt/keyrings
        state: directory
        mode: '0755'

    - name: Download Docker's official GPG key
      get_url:
        url: https://download.docker.com/linux/ubuntu/gpg
        dest: /etc/apt/keyrings/docker.asc
        mode: '0644'

    - name: Add Docker repository to apt sources list
      shell: |
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
      args:
        creates: /etc/apt/sources.list.d/docker.list

    - name: Update apt package index after adding Docker repository
      apt:
        update_cache: yes

    - name: Install Docker Engine
      apt:
        name: docker-ce
        state: present

    - name: Ensure Docker service is started and enabled
      service:
        name: docker
        state: started
        enabled: yes

    # - name: Verify Docker installation
    #   command: docker --version
    #   register: docker_version
    #   changed_when: false

    # - name: Display Docker version
    #   debug:
    #     msg: "Docker version installed: {{ docker_version.stdout }}"


    - name: Ensure the docker group exists
      group:
        name: docker
        state: present

    - name: Add the user to the docker group
      user:
        name: "{{ ansible_user }}"  # ubuntu
        group: docker
        append: yes    

```

