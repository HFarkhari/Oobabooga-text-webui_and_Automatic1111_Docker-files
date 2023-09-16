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
COPY launch.py .
COPY webui.py .
COPY webui.sh .


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


#WORKDIR /home/user/workspace
USER user


EXPOSE 7860
EXPOSE 5000  
EXPOSE 5005

#CMD ["python3","-u","launch.py", "--listen", "--port", "7860", "--api", "--xformers", "--enable-insecure-extension-access"]

CMD ["python3","-u","webui.py", "--listen", "--port", "7860", "--api", "--xformers", "--enable-insecure-extension-access"]

