FROM golang AS build-env
ADD . /go/src/app
WORKDIR /go/src/app

ENV http_proxy http://192.168.1.9:12333
ENV https_proxy http://192.168.1.9:12333

RUN go get -u -v github.com/kardianos/govendor
RUN govendor sync
RUN GOOS=linux GOARCH=arm GOARM=7 go build -v -o /go/src/app/app-server


    
FROM armhf/alpine:latest
RUN apk add -U tzdata
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
COPY --from=build-env /go/src/app/app-server /usr/local/bin/app-server
EXPOSE 8080
CMD [ "app-server" ]
