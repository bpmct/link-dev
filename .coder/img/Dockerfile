# See our guides for writing custom images:
# https://coder.com/docs/guides/customization

FROM codercom/enterprise-base:ubuntu

USER root

# Set NVM_DIR outside of the home directory so it doesn't persist across rebuilds
ENV NVM_DIR=/usr/bin/nvm
RUN mkdir -p $NVM_DIR
RUN chown coder:coder $NVM_DIR

# Install nvm as the "coder" user
USER coder
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    # Install any Node versions we need
    && nvm install 12.21.0 \
    && nvm install v16 \
    && nvm alias default v16 \
    && nvm use default \
    # Install global packages on the default version of node
    && npm install --global yarn
  
# Install go 1.17.1
RUN curl -L "https://dl.google.com/go/go1.17.1.linux-amd64.tar.gz" | tar -C /usr/local -xzvf -

# Setup go env vars
ENV GOROOT /usr/local/go
ENV PATH $PATH:$GOROOT/bin
ENV GOPATH /home/coder/go
ENV GOBIN $GOPATH/bin
ENV PATH $PATH:$GOBIN


# Copy configure script (we need this to add nvm to our path)
COPY configure /coder/configure
