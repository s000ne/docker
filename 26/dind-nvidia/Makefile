IMAGE=dind-nvidia
CONTAINER=dind

build:
	docker build . -t ${IMAGE}

run:
	docker run -d --rm  --name ${CONTAINER} --privileged ${IMAGE}

exec:
	docker exec -it ${CONTAINER} /bin/bash

stop:
	docker stop ${CONTAINER}