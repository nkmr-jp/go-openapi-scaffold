# 🚀 go-openapi-scaffold


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [go-openapi-scaffold](#go-openapi-scaffold)
  - [Prepare](#prepare)
  - [Getting Started](#getting-started)
    - [Step 1. get resources](#step-1-get-resources)
    - [Step 2. update spec](#step-2-update-spec)
    - [Step 3. generate api](#step-3-generate-api)
    - [Step 4. run api](#step-4-run-api)
    - [Step 5. show api document](#step-5-show-api-document)
  - [How to use in your project](#how-to-use-in-your-project)
    - [Step 1. get apigen](#step-1-get-apigen)
    - [Step 2. update api settings](#step-2-update-api-settings)

<!-- /code_chunk_output -->


## Prepare

```sh
brew install jq openapi-generator
brew install golangci/tap/golangci-lint
npm install -g redoc-cli
go get golang.org/x/tools/cmd/goimports
```
```sh
go version
# go version go1.14.7 darwin/amd64
jq --version
# jq-1.6
openapi-generator version
# 4.3.1
redoc-cli --version
# 0.9.12

# Other versions are not tested
```

## Getting Started

### Step 1. get resources
```
git clone https://github.com/nkmr-jp/go-openapi-scaffold
cd go-openapi-scaffold/apigen
make get-petstore-sample-spec
```

### Step 2. update spec

`configs/petstore.yml`
```diff
openapi: 3.0.0
servers:
-  - url: 'http://petstore.swagger.io/v2'
+  - url: 'http://localhost:8080'
```

### Step 3. generate api
```sh
make gen
```

### Step 4. run api
```sh
cd ../examples/petstore
go run main.go
# 2020/11/07 17:32:19 rotate.go:27: log file path: ./log/app_%Y-%m-%d.log
# 2020/11/07 17:32:19 logger.go:26: INFO INIT_LOGGER
# 2020/11/07 17:32:19 main.go:25: INFO SERVER_STARTED

# From Other Console
curl -i  http://localhost:8080/pet/1
# HTTP/1.1 200 OK
# Date: Sat, 07 Nov 2020 08:47:00 GMT
# Content-Length: 0
```

### Step 5. show api document
```sh
cd ../examples/petstore
make doc
# static html api document open in browser.
```

## How to use in your project

### Step 1. get apigen
```sh
cd [you project path]
svn checkout https://github.com/nkmr-jp/go-openapi-scaffold/trunk/apigen
cd apigen
rm .svn

git add .
git commit "Add apigen from https://github.com/nkmr-jp/go-openapi-scaffold"
```

### Step 2. update api settings

`apigen/Makefile`
```Makefile
# Update below settings to your own project
GIT_USER_ID=sample-git-user
GIT_REPO_ID=sample-repo-id

CONFIG=./configs/go-server-config.yaml
INPUT_SPEC=./configs/petstore.yml
OUTDIR=../examples/petstore
TEMPLATE_DIR=./templates
```

