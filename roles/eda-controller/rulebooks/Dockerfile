FROM quay.io/ansible/ansible-rulebook:latest

USER root

RUN pip install docker \
 && ansible-galaxy collection install community.docker -p /usr/share/ansible/collections --force
