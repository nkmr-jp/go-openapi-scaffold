# go-rest-apigen

## Prepare
```sh
brew install jq openapi-generator
npm install -g redoc-cli

jq --version
# jq-1.6
openapi-generator version
# 4.3.1
redoc-cli --version
# 0.9.12
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
