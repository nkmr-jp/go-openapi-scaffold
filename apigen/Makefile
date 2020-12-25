# API Settings

GIT_USER_ID=sample-git-user
GIT_REPO_ID=sample-repo-id

CONFIG=./configs/go-server-config.yaml
INPUT_SPEC=./configs/petstore.yml
OUTDIR=../examples/petstore
TEMPLATE_DIR=./templates

# Color settings
# See: https://gist.github.com/rsperl/d2dfe88a520968fbc1f49db0a29345b9
# define standard colors
BLACK        := $(shell tput -Txterm setaf 0)
RED          := $(shell tput -Txterm setaf 1)
GREEN        := $(shell tput -Txterm setaf 2)
YELLOW       := $(shell tput -Txterm setaf 3)
LIGHTPURPLE  := $(shell tput -Txterm setaf 4)
PURPLE       := $(shell tput -Txterm setaf 5)
BLUE         := $(shell tput -Txterm setaf 6)
WHITE        := $(shell tput -Txterm setaf 7)
RESET := $(shell tput -Txterm sgr0)

# Generate API
gen:
	@echo
	@echo "${GREEN}# GEN: 1. Generate Code (by openapi-generator)${RESET}"
	openapi-generator generate \
		--git-user-id=$(GIT_USER_ID) \
	   	--git-repo-id=$(GIT_REPO_ID) \
	   	--config=$(CONFIG) \
	   	-g go-server \
		-o $(OUTDIR) \
		-i $(INPUT_SPEC) \
		-t $(TEMPLATE_DIR)

	@echo
	@echo "${GREEN}# GEN: 2. Copy and Move and Format Code (by shell script)${RESET}"
	cp $(TEMPLATE_DIR)/.golangci.yml $(OUTDIR)/.golangci.yml
	cp $(TEMPLATE_DIR)/Makefile $(OUTDIR)/Makefile

	rm -rf $(OUTDIR)/internal/model
	mkdir -p $(OUTDIR)/internal $(OUTDIR)/internal/model $(OUTDIR)/internal/service
	#echo 'package service' > $(OUTDIR)/internal/service/service.go
	mv -v $(OUTDIR)/api/model_* $(OUTDIR)/internal/model
	rm $(OUTDIR)/api/logger.go

	grep -l 'model.string' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/model.string/string/g'
	grep -l 'model.int64' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/model.int64/int64/g'
	grep -l 'model._' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/model._/_/g'
	grep -l 'model.\[\]' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/model\.\[\]/\[\]/g'
	grep -l 'model.\*' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/model.\*/\*/g'
	grep -l '\[\]User' $(OUTDIR)/api/* | xargs sed -i.bak -e 's/\[\]User/[]model.User/g'

	rm $(OUTDIR)/api/*.bak

	@echo
	@echo "${GREEN}# GEN: 3. Install Vendor Code, and Remove Unused Packages (by go mod)${RESET}"
	cd $(OUTDIR); go mod download

	@echo
	@echo "${GREEN}# GEN: 4. Formant and Lint (by golang packages)${RESET}"
	# auto fix "imported but not used" error
	cd $(OUTDIR); goimports -w ./api/
	# auto fix code style
	cd $(OUTDIR); gofmt -s -w ./api/
	cd $(OUTDIR); gofmt -s -w ./internal/model
	cd $(OUTDIR); golangci-lint run --fix ./api

	@echo
	@echo "${GREEN}# GEN: 5. Generate Static HTML Document (by redoc-cli)${RESET}"
	cd $(OUTDIR)/api; redoc-cli bundle ./openapi.yaml

# Regenerate API
regen:
	@echo
	@echo "${GREEN}# REGEN: 1. Remove and Generate API (by shell and make gen)${RESET}"
	rm -rf $(OUTDIR)/api
	make gen

	@echo
	@echo "${GREEN}# REGEN: 2. Reset api/*_service.go (by git checkout)${RESET}"
	@echo "${YELLOW}Note: If you want to regenerate $(OUTDIR)/api/*_service.go too comment out below line${RESET}"
	git checkout $(OUTDIR)/api/*_service.go

# Download go-server templates from OpenAPITools
# Use when you want to reset template
# See: https://github.com/OpenAPITools/openapi-generator/tree/v4.3.1/modules/openapi-generator/src/main/resources/go-server
get-go-server-templates:
	@echo "${BLUE}# CHECKOUT TEMPLATES${RESET}"
	svn checkout \
		https://github.com/OpenAPITools/openapi-generator/tags/v4.3.1/modules/openapi-generator/src/main/resources/go-server \
		$(TEMPLATE_DIR)

# Download sample spec(petstore.yml) from OpenAPITools
# See: https://github.com/OpenAPITools/openapi-generator/blob/master/modules/openapi-generator/src/test/resources/3_0/petstore.yaml
get-petstore-sample-spec:
	curl -so $(INPUT_SPEC) \
	https://raw.githubusercontent.com/openapitools/openapi-generator/master/modules/openapi-generator/src/test/resources/3_0/petstore.yaml

