FROM diamol/node

CMD ["node", "/web-ping/app.js"]

ENV TARGET="www.naver.com" \
    METHOD="HEAD" \
    INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

