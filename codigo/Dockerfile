FROM nvidia/cuda:13.0.2-cudnn-devel-ubuntu24.04

ENV DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC MPLBACKEND=Agg

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 python3-pip python3-dev python3-venv build-essential git \
    pkg-config libffi-dev libssl-dev libxml2-dev libxslt1-dev \
    && rm -rf /var/lib/apt/lists/*

ARG USERNAME=dev
RUN useradd -m -s /bin/bash ${USERNAME}
USER ${USERNAME}
WORKDIR /home/${USERNAME}

COPY requirements.txt .

RUN python3 -m venv /home/$USERNAME/venv && \
    /home/$USERNAME/venv/bin/pip install --upgrade pip setuptools wheel && \
    /home/$USERNAME/venv/bin/pip install -r requirements.txt
ENV PATH="/home/${USERNAME}/venv/bin:$PATH"

RUN mkdir -p /home/${USERNAME}/notebooks

EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--NotebookApp.token=''", "--NotebookApp.password=''", "--NotebookApp.disable_check_xsrf=True", "--ServerApp.allow_origin='*'", "--ServerApp.trust_xheaders=True", "--NotebookApp.base_url=/"]
