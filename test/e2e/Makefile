COMETBFT_BUILD_OPTIONS += badgerdb,boltdb,cleveldb,rocksdb

include ../../common.mk

all: docker generator runner

docker:
	@echo "Building E2E Docker image"
	@docker build \
		--tag cometbft/e2e-node:local-version \
		-f docker/Dockerfile ../..

docker-debug:
	@echo "Building E2E Docker image for debugging"
	@docker build \
		--tag cometbft/e2e-node:local-version \
		-f docker/Dockerfile.debug ../..

docker-fast:
	@echo "Building fast-prototyping E2E Docker image"
	@docker build \
		--tag cometbft/e2e-node:local-version \
		-f docker/Dockerfile.fast .

# We need to build support for database backends into the app in
# order to build a binary with a CometBFT node in it (for built-in
# ABCI testing).
node:
	go build -race $(BUILD_FLAGS) -tags '$(BUILD_TAGS)' -o build/node ./node

# There is some redundancy between this and the ../../Makefile#build-linux
node-fast:
	CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o build/node ./node

generator:
	go build -o build/generator ./generator

runner:
	go build -o build/runner ./runner

.PHONY: all node docker generator runner 
