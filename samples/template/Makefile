# `Makefile` for local development.
# Use to build and run module container for your platform.

# module/container name
IMAGE_NAME := module

SHELL := /bin/bash
DOCKER_FILE := Dockerfile.x86
DOCKER_RUNTIME := "--gpus=all"
PROJECT_PATH := /opt/savant

ifeq ("$(shell uname -m)", "aarch64")
    DOCKER_FILE := Dockerfile.l4t
	DOCKER_RUNTIME := "--runtime=nvidia"
endif

# build module container
build:
	DOCKER_BUILDKIT=1 docker build \
		-f docker/$(DOCKER_FILE) \
		-t $(IMAGE_NAME) .

# run module
run:
	docker run -it --rm $(DOCKER_RUNTIME) \
		-v $(shell pwd)/models_cache:/models \
		-v $(shell pwd)/downloads_cache:/downloads \
		$(IMAGE_NAME)

# run module in dev mode
run-dev:
	xhost +local:docker
	docker run -it --rm $(DOCKER_RUNTIME) \
		--net=host --privileged \
		-e DISPLAY=$(DISPLAY) \
		-e XAUTHORITY=/tmp/.docker.xauth \
		-v /tmp/.X11-unix:/tmp/.X11-unix \
		-v /tmp/.docker.xauth:/tmp/.docker.xauth \
		-v $(shell pwd)/models_cache:/models \
		-v $(shell pwd)/downloads_cache:/downloads \
		-v $(shell pwd)/module:$(PROJECT_PATH)/module \
		--entrypoint /bin/bash \
		$(IMAGE_NAME)
