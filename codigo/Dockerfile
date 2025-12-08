FROM nvidia/cuda:12.9.0-cudnn-devel-ubuntu24.04

ENV DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC MPLBACKEND=Agg

# Usamos un snapshot de los repositorios, para que siempre se tomen los mismos paquetes
RUN sed -i 's|http://archive.ubuntu.com/ubuntu/|http://snapshot.ubuntu.com/ubuntu/20250507T091500Z/|g' /etc/apt/sources.list \
    && sed -i 's|http://security.ubuntu.com/ubuntu/|http://snapshot.ubuntu.com/ubuntu/20250507T091500Z/|g' /etc/apt/sources.list

# Instalamos lo básico
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 python3-pip python3-dev python3-venv \
    build-essential git pkg-config \
    libffi-dev libssl-dev libxml2-dev libxslt1-dev \
    && rm -rf /var/lib/apt/lists/*

# Usuario y directorio de trabajo
ARG USERNAME=ubuntu
USER ${USERNAME}
WORKDIR /home/${USERNAME}

# Dependencias versionadas Python en un ambiente virtual
COPY requirements.txt .
RUN python3 -m venv /home/$USERNAME/venv && \
    /home/$USERNAME/venv/bin/pip install --upgrade pip setuptools wheel && \
    /home/$USERNAME/venv/bin/pip install -r requirements.txt
ENV PATH="/home/${USERNAME}/venv/bin:$PATH"

# Crea directorio a montar
RUN mkdir -p /home/${USERNAME}/notebooks
RUN chown -R ${USERNAME} /home/${USERNAME}/notebooks

# Puerto
EXPOSE 8888

# Jupyter
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--NotebookApp.token=''", "--NotebookApp.password=''", "--NotebookApp.disable_check_xsrf=True", "--ServerApp.allow_origin='*'", "--ServerApp.trust_xheaders=True", "--NotebookApp.base_url=/"]
