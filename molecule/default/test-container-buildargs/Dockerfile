FROM alpine:latest

MAINTAINER "Michael Buluma"

ARG test_var

# Test that build arg was passed correctly.
RUN if [ "x$test_var" = "x" ] ; then echo "Build arg not provided" && exit 1 ; fi

CMD ["/bin/sh"]
