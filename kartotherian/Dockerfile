FROM node:6-stretch@sha256:9b5d8caed98111c7bcacd8c46bce19635670b04d59dba20f765b096d6ef98dea

ENV NODE_ENV=production

# Upgrade npm
RUN yarn global add npm \
    && mkdir -p /etc/kartotherian \
    && chown node /etc/kartotherian \
    && chown node /opt

USER node

RUN git clone https://github.com/QwantResearch/kartotherian.git /opt/kartotherian \
    && cd /opt/kartotherian \
    && git checkout b021dde5ac37 \
    && npm install --production \
    && git clone https://github.com/QwantResearch/kartotherian_config.git /opt/kartotherian_config \
    && cd /opt/kartotherian_config \
    && git checkout 49ed34abc4ed1241f14fb0c7edf5dfe86cd0821c

COPY --chown=node variables.yaml /opt/kartotherian
COPY --chown=node config.yaml /opt/kartotherian

RUN chmod 600 /opt/kartotherian/variables.yaml \
    && mkdir -p /etc/kartotherian\
    && ln -sf /opt/kartotherian/config.yaml /etc/kartotherian\
    && ln -sf /opt/kartotherian/variables.yaml /etc/kartotherian\
    && ln -sf /opt/kartotherian_config/kartotherian/sources.yaml /etc/kartotherian

CMD node /opt/kartotherian/server.js -c /etc/kartotherian/config.yaml
