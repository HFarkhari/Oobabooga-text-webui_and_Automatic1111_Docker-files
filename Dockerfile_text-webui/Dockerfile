FROM python:3.10-slim

ENV PATH /usr/local/bin:$PATH
ENV LANG C.UTF-8
ENV PYTHONUNBUFFERED=1
ENV PYTHONIOENCODING=UTF-8


RUN apt-get update && \
    apt-get clean all && \
    apt-get install --no-install-recommends -y build-essential \
    ffmpeg libsm6 libxext6 \
    wget curl unzip git nano \
    libgl1-mesa-glx libglib2.0-0 libssl-dev && \
    apt-get clean && \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false && \
    pip install --no-cache-dir --upgrade pip && \
    pip cache purge && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

COPY requirements_nover_upgrade.txt .
COPY server.py .


RUN  pip install -r requirements_nover_upgrade.txt && \
     pip cache purge && \
     rm -rf /tmp/*

RUN useradd -m -s /bin/bash user
WORKDIR /workspace
ENV TRANSFORMERS_CACHE=/workspace/cache
ENV CACHE_DIR=/workspace/cache
ENV HOME=/workspace/user
ENV XDG_CONFIG_HOME=$HOME/.config
ENV XDG_CACHE_HOME=$CACHE_DIR

USER user


EXPOSE 7777
EXPOSE 5000  
EXPOSE 5005  

CMD ["python", "server.py", "--listen", "--api", "--listen-port", "7777"]

