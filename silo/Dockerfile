ARG BUILD_FROM
FROM ${BUILD_FROM}

# Build arguments
ARG BUNDLER_VERSION=2.5.0
ARG SILO_VERSION=main

# Environment setup
ENV RAILS_ENV=production \
    BUNDLE_DEPLOYMENT=1 \
    BUNDLE_PATH=/usr/local/bundle \
    BUNDLE_WITHOUT=development:test \
    RAILS_SERVE_STATIC_FILES=1 \
    RAILS_LOG_TO_STDOUT=1 \
    DISABLE_SSL=true \
    SOLID_QUEUE_IN_PUMA=true \
    PORT=8099 \
    LANG=C.UTF-8

# Install Ruby and build dependencies from Alpine packages
# Using Alpine's packaged Ruby for cross-platform compatibility
RUN apk add --no-cache \
    ruby \
    ruby-dev \
    ruby-bundler \
    ruby-json \
    ruby-bigdecimal \
    ruby-etc \
    build-base \
    gcc \
    g++ \
    make \
    libc-dev \
    libffi-dev \
    libxml2-dev \
    libxslt-dev \
    yaml-dev \
    zlib-dev \
    sqlite \
    sqlite-dev \
    libxml2 \
    libxslt \
    tzdata \
    curl \
    jq \
    imagemagick \
    vips \
    vips-dev \
    nodejs \
    npm \
    git \
    bash \
    openssl

# Update bundler to specific version
RUN gem install bundler -v ${BUNDLER_VERSION} --no-document

# Clone the Silo application (public repo)
WORKDIR /rails
RUN git clone --depth 1 --branch ${SILO_VERSION} https://github.com/kebabmane/silo-rss.git . && \
    rm -rf .git

# Install gems
RUN bundle config set --local deployment true && \
    bundle config set --local without 'development test' && \
    bundle install --jobs $(nproc) && \
    rm -rf ~/.bundle/ "${BUNDLE_PATH}"/ruby/*/cache "${BUNDLE_PATH}"/ruby/*/bundler/gems/*/.git

# Precompile bootsnap
RUN bundle exec bootsnap precompile --gemfile app/ lib/ || true

# Precompile assets (requires dummy keys)
RUN SECRET_KEY_BASE_DUMMY=1 ./bin/rails assets:precompile || true

# Cleanup build dependencies to reduce image size
RUN apk del build-base gcc g++ make ruby-dev libffi-dev libxml2-dev libxslt-dev yaml-dev zlib-dev sqlite-dev vips-dev git && \
    rm -rf /tmp/* /var/cache/apk/*

# Copy rootfs overlay
COPY rootfs /

# Make scripts executable
RUN chmod +x /usr/bin/silo-init && \
    chmod +x /etc/s6-overlay/s6-rc.d/silo/run && \
    chmod +x /etc/s6-overlay/s6-rc.d/silo/finish

# Build arguments for labels
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Rhys Evans" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
