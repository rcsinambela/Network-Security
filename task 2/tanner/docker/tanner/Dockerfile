FROM alpine:3.15

# Include dist
ADD dist/ /root/dist/
RUN mkdir /opt/tanner-copy
COPY tanner-copy/ /opt/tanner-copy

# Setup apt
RUN apk -U --no-cache add \
    build-base \
    git \
    nano \
    libcap \
    libffi-dev \
    libressl-dev \
    linux-headers \
    py3-yarl \
    python3 \
    python3-dev \
    py3-pip && \
# Setup Tanner
    git clone --depth=1 https://github.com/mushorg/tanner /opt/tanner && \
    cp /root/dist/config.yaml /opt/tanner/tanner/data/ && \
    cp opt/tanner-copy/requirements.txt /opt/tanner && \
    cp opt/tanner-copy/setup.py /opt/tanner && \
    cp opt/tanner-copy/aiodocker_helper.py /opt/tanner/tanner/utils && \
    cp opt/tanner-copy/api.py /opt/tanner/tanner/api && \
    cp opt/tanner-copy/session_analyzer.py /opt/tanner/tanner/sessions && \
    cd /opt/tanner/ && \
    python3 -m venv tanner-env && \
    source /opt/tanner/tanner-env/bin/activate && \
    pip install --upgrade pip && \
    pip3 install --no-cache-dir wheel && \
    pip3 install --no-cache-dir -r requirements.txt && \
    python3 setup.py install && \
    cd / && \
# Setup configs, user, groups
    chown -R nobody:nobody /opt/tanner && \
# Clean up
    apk del --purge \
            build-base \
            linux-headers \
            python3-dev && \
    rm -rf /root/* && \
    rm -rf /tmp/* /var/tmp/* && \
    rm -rf /var/cache/apk/*

# Start tanner
USER nobody:nobody
WORKDIR /opt/tanner
CMD ["/opt/tanner/tanner-env/bin/tanner", "--config", "/opt/tanner/tanner/data/config.yaml"]

