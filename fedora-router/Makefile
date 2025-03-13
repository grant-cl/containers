REGISTRY := registry.hpg.nz
IMAGE_NAME := fedora/router-bootc
FILE_NAME := fedora-router

.PHONY: all clean container boot boot-iso

all: container boot boot-iso

container:
	podman build -t "$(IMAGE_NAME)" -f Containerfile
	podman tag "$(IMAGE_NAME)" "$(REGISTRY)/$(IMAGE_NAME)"
	podman push "$(REGISTRY)/$(IMAGE_NAME)"

boot:
	sudo podman pull "$(REGISTRY)/$(IMAGE_NAME)":latest
	if [ ! -d output ]; then mkdir output; fi
	sudo podman run \
		--rm \
		-it \
		--privileged \
		--pull=newer \
		--security-opt label=type:unconfined_t \
		-v ./config.toml:/config.toml:ro \
		-v ./output:/output \
		-v /var/lib/containers/storage:/var/lib/containers/storage \
		quay.io/centos-bootc/bootc-image-builder:latest \
		--config /config.toml \
		--type qcow2 \
		--rootfs xfs \
		--use-librepo=True \
		"$(REGISTRY)/$(IMAGE_NAME)":latest
	sudo chown -R "$(USER):$(USER)" output/
	mv output/qcow2/disk.qcow2 output/qcow2/$(FILE_NAME)-$(shell date +%Y%m%d%H%M).qcow2

boot-iso:
	sudo podman pull "$(REGISTRY)/$(IMAGE_NAME)":latest
	if [ ! -d output ]; then mkdir output; fi
	sudo podman run \
		--rm \
		-it \
		--privileged \
		--pull=newer \
		--security-opt label=type:unconfined_t \
		-v ./config.toml:/config.toml:ro \
		-v ./output:/output \
		-v /var/lib/containers/storage:/var/lib/containers/storage \
		quay.io/centos-bootc/bootc-image-builder:latest \
		--config /config.toml \
		--type anaconda-iso \
		--rootfs lvm \
		--use-librepo=True \
		"$(REGISTRY)/$(IMAGE_NAME)":latest



clean:
	sudo podman rmi "$(IMAGE_NAME)" "$(REGISTRY)/$(IMAGE_NAME)" || true
	rm -rf output/*
