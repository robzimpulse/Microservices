build:
	protoc -I. --go_out=plugins=grpc:$(GOPATH)/src/Microservices/consignment-service \
	  proto/consignment/consignment.proto
	docker build -t consignment-service .

run:
    docker run -d 50051:50051 consignment-service