DOCKER_FILE=Dockerfile.deepo_base_cpu
CONTAINER_NAME="deepo-base-cpu"
IMAGE_NAME=deepo-base-cpu
DOCKER=docker
#REPO_ROOT?=$(shell dirname `pwd`)
#DOCKER_ENV_REPO_ROOT=/camera_projection
#$(info $$REPO_ROOT is [${REPO_ROOT}])
#$(info $$DOCKER_ENV_REPO_ROOT is [${DOCKER_ENV_REPO_ROOT}])

X11_DISPLAY=--env DISPLAY=$(DISPLAY) \
	--env="QT_X11_NO_MITSHM=1" \
	-v /tmp/.X11-unix:/tmp/.X11-unix:ro

## Modified by Kevin
MOUNT_DIR= -v /home/kai7/data:/data_host 

MOUNT_WEBCAM=--device /dev/video0:/dev/video0


.PHONY: build bash x11 start exec

build:
	$(DOCKER) build -t $(IMAGE_NAME) -f $(DOCKER_FILE) . --no-cache

bash:
	#$(DOCKER) run -it -w /workspace --privileged --rm $(MOUNT_DIR) $(MOUNT_WEBCAM) --net=host $(IMAGE_NAME) bash
	#$(DOCKER) run -it -w $(DOCKER_ENV_REPO_ROOT) --privileged --rm $(MOUNT_DIR) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash
	$(DOCKER) run -it -w /workspace --privileged --rm $(MOUNT_DIR) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash

x11:
	xhost +
	#$(DOCKER) run -it -w /workspace --privileged --rm $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) --net=host $(IMAGE_NAME) bash
	#$(DOCKER) run -it -w $(DOCKER_ENV_REPO_ROOT) --privileged --rm $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash
	$(DOCKER) run -it -w /workspace --privileged --rm $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash
	xhost -

start: 
	xhost +
ifeq ($(shell docker ps -aqf name=${CONTAINER_NAME}),)
	#$(DOCKER) run -it -w /workspace --privileged -u $(id -u):$(id -g) --name ${CONTAINER_NAME} $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) --net=host $(IMAGE_NAME) bash
	#$(DOCKER) run -it -w $(DOCKER_ENV_REPO_ROOT) --privileged -u $(id -u):$(id -g) --name ${CONTAINER_NAME} $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash
	$(DOCKER) run -it -w /workspace --privileged -u $(id -u):$(id -g) --name ${CONTAINER_NAME} $(MOUNT_DIR) $(X11_DISPLAY) $(MOUNT_WEBCAM) $(IMAGE_NAME) bash
else
	$(DOCKER) start -ai ${CONTAINER_NAME}
endif
	xhost -

exec:
	$(DOCKER) exec -it $(CONTAINER_NAME) bash
