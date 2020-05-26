FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

ENV OPERATOR=/usr/local/bin/hello-demo-operator \
    USER_UID=1001 \
    USER_NAME=hello-demo-operator

# install operator binary
COPY build/_output/bin/hello-demo-operator ${OPERATOR}

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
