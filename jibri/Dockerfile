ARG JITSI_REPO=jitsi
ARG BASE_TAG=latest
FROM ${JITSI_REPO}/base-java:${BASE_TAG}

#ARG CHROME_RELEASE=latest
#ARG CHROMEDRIVER_MAJOR_RELEASE=latest
ARG CHROME_RELEASE=94.0.4606.61
ARG CHROMEDRIVER_MAJOR_RELEASE=94

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y jibri libgl1-mesa-dri procps && \
    apt-cleanup && \
    [ "${CHROME_RELEASE}" = "latest" ] && \
    wget -qO - https://dl-ssl.google.com/linux/linux_signing_key.pub | gpg --dearmour /etc/apt/trusted.gpg.d/google.gpg && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list && \
    apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y google-chrome-stable && \
    apt-cleanup || \
    [ "${CHROME_RELEASE}" != "latest" ] && \
    curl -4so "/tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb" "http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb" && \
    apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y "/tmp/google-chrome-stable_${CHROME_RELEASE}-1_amd64.deb" && \
    apt-cleanup || \
    [ "${CHROMEDRIVER_MAJOR_RELEASE}" = "latest" ] && \
    CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE)" || \
    CHROMEDRIVER_RELEASE="$(curl -4Ls https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROMEDRIVER_MAJOR_RELEASE})" && \
    curl -4Ls "https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_RELEASE}/chromedriver_linux64.zip" \
    | zcat >> /usr/bin/chromedriver && \
    chmod +x /usr/bin/chromedriver && \
    chromedriver --version && \
    apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y jitsi-upload-integrations jq && \
    apt-cleanup

COPY rootfs/ /

VOLUME /config
