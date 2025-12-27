.PHONY: init run stop clean attach exec

CONTAINER_NAME = openwrt-builder
# Base image from Docker Hub
BASE_IMAGE = docker.io/openwrt/sdk:x86-64
# Custom image with hapcan branch
CUSTOM_IMAGE = openwrt-hapcan-builder
OPENWRT_DIR = $(shell dirname $(CURDIR))
BUILD_DIR = $(CURDIR)/build
UID = $(shell id -u)
GID = $(shell id -g)

init:
	@echo "Building custom image with hapcan branch..."
	podman build -t $(CUSTOM_IMAGE) -f Dockerfile .
	@echo "Custom image ready!"

run:
	@echo "Starting OpenWrt build container..."
	@mkdir -p $(BUILD_DIR)
	podman run -it --name $(CONTAINER_NAME) \
		-v $(BUILD_DIR):/home/buildbot/build \
		$(CUSTOM_IMAGE) /bin/bash

stop:
	@echo "Stopping container..."
	podman stop $(CONTAINER_NAME) || true

clean: stop
	@echo "Removing container..."
	podman rm -f $(CONTAINER_NAME) || true
	@echo "Cleanup complete"

attach:
	@echo "Attaching to running container..."
	podman attach $(CONTAINER_NAME)

exec:
	@echo "Opening new shell in container..."
	podman exec -it --user $(UID):0 $(CONTAINER_NAME) /bin/bash

help:
	@echo "OpenWrt Podman Build Environment"
	@echo ""
	@echo "Available targets:"
	@echo "  make run     - Start the build container (interactive)"
	@echo "  make stop    - Stop the running container"
	@echo "  make clean   - Remove container"
	@echo "  make attach  - Attach to running container"
	@echo "  make exec    - Open new shell in running container"
	@echo ""
	@echo "Uses official OpenWrt buildenv image from docker.io/openwrt/buildenv"
