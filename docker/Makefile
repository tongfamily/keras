help:
	@cat Makefile

DATA?="${HOME}/Data"
GPU?=0
DOCKER_FILE=Dockerfile
DOCKER=GPU=$(GPU) nvidia-docker
BACKEND=tensorflow
PYTHON_VERSION?=3.6
CUDA_VERSION?=9.0
CUDNN_VERSION?=7
TEST=tests/
SRC?=$(shell dirname `pwd`)
DOCKER_ORG=keras
DOCKER_REPO=keras

build:
	docker build -t $(DOCKER_ORG)/$(DOCKER_REPO) --build-arg python_version=$(PYTHON_VERSION) --build-arg cuda_version=$(CUDA_VERSION) --build-arg cudnn_version=$(CUDNN_VERSION) -f $(DOCKER_FILE) .

bash: build
	$(DOCKER) run -it -v $(SRC):/src/workspace -v $(DATA):/data --env KERAS_BACKEND=$(BACKEND) keras bash

ipython: build
	$(DOCKER) run -it -v $(SRC):/src/workspace -v $(DATA):/data --env KERAS_BACKEND=$(BACKEND) keras ipython

notebook: build
	$(DOCKER) run -it -v $(SRC):/src/workspace -v $(DATA):/data -p 8888:8888 --env KERAS_BACKEND=$(BACKEND) keras
	# this works only for linux not for macos
	# $(DOCKER) run -it -v $(SRC):/src/workspace -v $(DATA):/data -p 8888:8888 --net=host --env KERAS_BACKEND=$(BACKEND) keras

test: build
	$(DOCKER) run -it -v $(SRC):/src/workspace -v $(DATA):/data --env KERAS_BACKEND=$(BACKEND) keras py.test $(TEST)

