# FROM fuzzers/go-fuzz:1.2.0
FROM golang:1.18 as builder

COPY . /terraform-json
WORKDIR /terraform-json/mayhem

RUN go install github.com/dvyukov/go-fuzz/go-fuzz@latest github.com/dvyukov/go-fuzz/go-fuzz-build@latest
RUN go get github.com/dvyukov/go-fuzz/go-fuzz-dep
RUN apt update && apt install -y clang


RUN go-fuzz-build -libfuzzer -o fuzzExpression.a && \
    clang -fsanitize=fuzzer fuzzExpression.a -o fuzzExpression.libfuzzer

FROM debian:bookworm-slim
COPY --from=builder /terraform-json/mayhem/fuzzExpression.libfuzzer /