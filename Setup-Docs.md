Wiki for setting up Dalal Street Server locally.

## Installing Go

Dala Street Server currently uses Golang 1.16. Download the same from this [link](https://golang.org/dl/go1.16.9.linux-amd64.tar.gz).

Follow this [link](https://golang.org/doc/install) to setup Golang. After this you should get `go version go1.16.9` upon running `go version`.

Golang needs 2 path variables for it to locate and store dependencies and executables

__$GOPATH__: Directory where all packages and modules for Go projects are located. 

__$GOBIN__: Directory where binary executables of Go projects are located.

Create a directory `go/` wherever you want and setup $GOPATH and $GOBIN paths in _.bashrc_ file.$GOBIN also need to be appended to $PATH variable

``` bash
export PATH=$PATH:`$OTHER_STUFF`:$GOBIN
export GOPATH=$HOME/go
export GOBIN=$GOPATH/bin

```

All Go packages will be stored under `$GOPATH/src` and modules under `$GOPATH/pkg/mod` and executables under `$GOBIN`

## Installing Protocol Buffer 

Protocol buffers [Download link](https://github.com/google/protobuf/releases/download/v3.2.0rc2/protoc-3.2.0rc2-linux-x86_64.zip)

Follow this [link](https://grpc.io/docs/protoc-installation/#install-pre-compiled-binaries-any-os) for setting up Protobuf.

To check if protobuf is installed properly run the following command.

``` bash
protoc --version
```

## Setting up Server Repo

 
``` bash
go get github.com/delta/dalal-street-server
```

Binary executables
``` bash
go install -tags 'mysql' github.com/golang-migrate/migrate/v4/cmd/migrate@latest
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1
```

Required Modules
```
cd $GOPATH/src/github.com/delta/dalal-street-server/
go get ./...
```

## Building Proto files

Proto files have to be built and converted to .pb.go files so that it can be used imported and used in our .go files

Go to dalal-street-server root directory and run this command
```
./build_proto.sh
```

## Migrations

### To run all the migrations
```
migrate -path "./migrations" -database "mysql://root:YOUR_MYSQL_PASSWORD@/dalalstreet_dev" up
```

### Create Migrations
```
migrate create -ext sql -dir ./migrations MIGRATION_NAME
```

## TLS keys
To create TLS keys for docker
```bash
mkdir tls_keys/docker/
cd tls_keys/docker/
openssl req -x509 -out server.crt -keyout server.key   -newkey rsa:2048 -nodes -sha256   -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
cd ../../

```