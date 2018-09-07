# STEP 1 build executable binary
FROM golang:alpine as builder

# Install SSL
RUN apk update && apk add git

# Create appuser
RUN adduser -D -g '' appuser

# Copy our project and make default working directory
COPY . $GOPATH/src/app/
WORKDIR $GOPATH/src/app/

# Get dependancies
RUN go get -u github.com/golang/dep/cmd/dep
RUN dep ensure -vendor-only

# Build the binary
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -ldflags='-w -s' -o /go/bin/app

## STEP 2 build a small image start from scratch
FROM scratch

# Copy our user data
COPY --from=builder /etc/passwd /etc/passwd

# Copy our static executable
COPY --from=builder /go/bin/app /go/bin/app

# Set current user
USER appuser

# Set entry point
ENTRYPOINT ["/go/bin/app"]


