

# https://mcr.microsoft.com/v2/dotnet/sdk/tags/list
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0.100-1 AS builder
ARG TARGETARCH

WORKDIR /usr/src/app/

COPY ./src/cartservice/ ./
COPY ./pb/ ./src/protos/

RUN dotnet restore ./src/cartservice.csproj -v d -r linux-musl-$TARGETARCH

RUN dotnet publish ./src/cartservice.csproj -v d -r linux-musl-$TARGETARCH --no-restore -o /cartservice

# -----------------------------------------------------------------------------

# https://mcr.microsoft.com/v2/dotnet/runtime-deps/tags/list
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0.0-alpine3.18

WORKDIR /usr/src/app/
COPY --from=builder /cartservice/ ./

ENV DOTNET_HOSTBUILDER__RELOADCONFIGONCHANGE=false

EXPOSE ${CART_SERVICE_PORT}
ENTRYPOINT [ "./cartservice" ]
