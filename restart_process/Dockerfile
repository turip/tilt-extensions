FROM alpine as entr-build

RUN apk update && apk add make git build-base

RUN git clone https://github.com/eradman/entr.git /entr
WORKDIR /entr
RUN git checkout e3cc0b101ac1fd714720e17f068604b8a696a874
RUN cp Makefile.linux Makefile
RUN CFLAGS="-static" make install

FROM golang as go-build
WORKDIR /src
ADD ./tilt-restart-wrapper.go ./tilt-restart-wrapper.go
RUN GO111MODULE=auto go build ./tilt-restart-wrapper.go

FROM scratch

COPY --from=entr-build /usr/local/bin/entr /
COPY --from=go-build /src/tilt-restart-wrapper /
