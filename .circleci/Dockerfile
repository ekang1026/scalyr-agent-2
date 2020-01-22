FROM centos as base

ARG PYTHON_VERSION

RUN yum install -y sudo gcc gcc-c++ make git patch openssl-devel zlib-devel readline-devel sqlite-devel bzip2-devel libffi-devel glibc-devel

RUN useradd -ms /bin/bash scalyr \
&& echo "scalyr:scalyr" | chpasswd \
&& usermod -aG wheel scalyr \
&& echo "scalyr ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER scalyr
WORKDIR /home/scalyr

RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv \
&& echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc \
&& echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc \
&& echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc


RUN . ~/.bashrc && pyenv install $PYTHON_VERSION


RUN source ~/.bashrc && pyenv shell $PYTHON_VERSION && pyenv version && pip install mock ujson docker-py==1.10.3


FROM centos

ARG PYTHON_VERSION

RUN useradd -ms /bin/bash scalyr \
&& echo "scalyr:scalyr" | chpasswd \
&& usermod -aG wheel scalyr \
&& echo "scalyr ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

COPY --from=base /home/scalyr/.pyenv /home/scalyr/.pyenv
COPY --from=base /home/scalyr/.bashrc /home/scalyr/.bashrc

USER scalyr
WORKDIR /home/scalyr

VOLUME ["/scalyr-agent-2"]

ENV PYTHON_VERSION=$PYTHON_VERSION


ENTRYPOINT source ~/.bashrc && pyenv shell $PYTHON_VERSION && pyenv version && python /scalyr-agent-2/run_tests.py