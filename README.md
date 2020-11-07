# go-openapi-scaffold

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
```

## Install
```sh

```

## Getting started
```
make 
```

## Usage

### Generate API
```sh
make gen 
```

### Regenerate API
delete and generate API
```sh
make regen
```

### Start API
```sh
make start
```

### Log Stream
```sh
make logs LEVEL=ERROR
```

### Show Last error details
```sh
make inspect
```

### Show OpenAPI Static Document in browser
```sh
make doc
```
