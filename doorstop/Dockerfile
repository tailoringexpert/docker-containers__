FROM bitnami/python:3.11

ENV VIRTUAL_ENV=/opt/venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
ENV QT_DEBUG_PLUGINS=1
ENV QT_QPA_PLATFORM=xcb

USER 0

RUN addgroup --gid 1001 doorstop && \
    adduser --uid 1001 --gid 1001 --disabled-password --gecos "" doorstop

RUN install_packages procps less vim gpg-agent\
                     libgl1 libegl1 libxkbcommon-tools libfontconfig1\
					 libdbus-1-3 libnss3 libxcomposite1 libxrender1 libxrandr2 libasound2 libxtst6 libxkbfile1\
					 libxcb-cursor-dev libxcb-icccm4 libxcb-keysyms1 libxcb-randr0 libxcb-shape0  
ADD rootfs/usr/local/bin /usr/local/bin

RUN python3 -m venv $VIRTUAL_ENV
RUN . /opt/venv/bin/activate && pip install  plantuml-markdown PySide6  doorstop doorstop-edit

USER doorstop
ENTRYPOINT /bin/bash
