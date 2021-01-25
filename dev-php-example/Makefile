DIR = .
FILE = Dockerfile
IMAGE = mwoodpatrick/php-fpm-dev
TAG = latest

.PHONY: build rebuild test tag pull login push enter

build:
	docker build -t $(IMAGE) -f $(DIR)/$(FILE) $(DIR)

rebuild: pull
	docker build --no-cache -t $(IMAGE) -f $(DIR)/$(FILE) $(DIR)

test:
	./tests/test.sh $(IMAGE)

tag:
	docker tag $(IMAGE) $(IMAGE):$(TAG)

pull:
	docker pull $(shell grep FROM Dockerfile | sed 's/^FROM//g';)

update-readme:
	cat "./README.md" \
		| perl -00 -pe "s/<!-- modules -->.*<!-- \/modules -->/<!-- modules -->\n$$(./tests/get-modules.sh)\n<!-- \/modules -->/s" \
		> "./README.md.tmp"
	yes | mv -f "./README.md.tmp" "./README.md"

login:
	yes | docker login --username $(USER) --password $(PASS)

push:
	@$(MAKE) tag TAG=$(TAG)
	docker push $(IMAGE):$(TAG)

enter:
	docker run --rm --name $(subst /,-,$(IMAGE)) -it --entrypoint=bash -p 127.0.0.1:5900:5900 $(ARG) $(IMAGE)
