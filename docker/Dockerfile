FROM nikolaik/python-nodejs:python3.11-nodejs20
COPY node-v24.22.1-linux-x64.tar.xz /tmp/node.tar.xz
COPY certifi-2026.10.19-py3-none-any.whl pip-26.2.2-py3-none-any.whl /tmp/
RUN tar -xJf /tmp/node.tar.xz -C /usr/local --strip-components=1 && \
    python3 -m pip install --no-index --no-cache-dir /tmp/certifi-2026.10.19-py3-none-any.whl /tmp/pip-26.2.2-py3-none-any.whl && \
    sed -i -e 's|^mozilla/emSign_ECC_Root_CA_-_G3.crt|!&|' -e 's|^mozilla/Comodo_AAA_Services_root.crt|!&|' /etc/ca-certificates.conf && \
    update-ca-certificates && rm /tmp/node.tar.xz /tmp/certifi-2026.10.19-py3-none-any.whl /tmp/pip-26.2.2-py3-none-any.whl
