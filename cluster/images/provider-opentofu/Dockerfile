FROM alpine:3.22.1
RUN apk --no-cache add ca-certificates bash git curl
ARG TARGETOS
ARG TARGETARCH

ENV OPENTOFU_VERSION=1.10.5
ENV TF_IN_AUTOMATION=1
ENV TF_PLUGIN_CACHE_DIR=/tofu/plugin-cache

ADD "bin/${TARGETOS}_${TARGETARCH}/provider" /usr/local/bin/crossplane-opentofu-provider
ADD .gitconfig .gitconfig

RUN curl -s -L https://github.com/opentofu/opentofu/releases/download/v${OPENTOFU_VERSION}/tofu_${OPENTOFU_VERSION}_${TARGETOS}_${TARGETARCH}.zip -o tofu.zip \
  && unzip -d /usr/local/bin tofu.zip \
  && rm tofu.zip \
  && chmod +x /usr/local/bin/tofu \
  && mkdir -p ${TF_PLUGIN_CACHE_DIR} \
  && chown -R 2000 /tofu
# As of Crossplane v1.3.0 provider controllers run as UID 2000.
# https://github.com/crossplane/crossplane/blob/v1.3.0/internal/controller/pkg/revision/deployment.go#L32

RUN mkdir -p /home/user2000 \
  && chown 2000:2000 /home/user2000 \ 
  && adduser -u 2000 -d /home/user2000 -m user2000
  
USER 65532
ENTRYPOINT ["crossplane-opentofu-provider"]
