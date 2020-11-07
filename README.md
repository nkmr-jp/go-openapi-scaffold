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

## Getting started
```
git clone https://github.com/nkmr-jp/go-openapi-scaffold
cd ../apigen
make get-petstore-sample-spec
make gen
cd ../../e
```


## Install to your project
```sh
cd [you project path]
svn checkout https://github.com/nkmr-jp/go-openapi-scaffold/trunk/apigen
cd apigen
rm .svn

git add .
git commit "Add apigen from https://github.com/nkmr-jp/go-openapi-scaffold"
```

