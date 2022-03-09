FROM nvcr.io/nvidia/tensorflow:21.03-tf1-py3
# FROM nvcr.io/nvidia/tensorflow:20.03-tf1-py3

ENV USER=dev
ENV UID=1001
ENV GID=1001

# Proxy Configure
RUN echo 'Acquire::http::proxy "http://proxy.uec.ac.jp:8080/";' >> /etc/apt/apt.conf && echo 'Acquire::https::proxy "http://proxy.uec.ac.jp:8080/";' >> /etc/apt/apt.conf

# synbolic link to avoid hanging
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Language setting
RUN echo 'export LANG=ja_JP.UTF-8' >> /root/.bashrc && echo 'export LANGUAGE="ja_JP:ja"' >> /root/.bashrc

USER root
RUN cat /etc/lsb-release
ENV HTTP_PROXY http://proxy.uec.ac.jp:8080/
ENV HTTPS_PROXY http://proxy.uec.ac.jp:8080/
RUN apt update && apt upgrade -y && apt install -y tzdata software-properties-common openssh-server



RUN add-apt-repository universe
# RUN apt update
RUN apt update && apt install -y \
    apt-utils build-essential git \
    bash-completion gnupg2 curl apt-transport-https \
    nano tree python3-pip bzip2-doc \
    libncurses-dev libbz2-dev libreadline-dev \
    libsqlite3-dev libssl-dev zlib1g-dev wget \
    llvm libncurses5-dev libncursesw5-dev \
    xz-utils tk-dev libffi-dev liblzma-dev \
    python-openssl openssh-server sudo
    # language-pack-ja-base language-pack-ja ibus-mozc
# RUN apt install -y openssh-server

# SSH setting --------->
USER root
RUN mkdir /var/run/sshd
RUN echo 'root:screencast' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
RUN echo "export VISIBLE=now" >> /etc/profile

RUN echo "export PATH=$PATH" >> /etc/profile && \
    echo "ldconfig" >> /etc/profile

WORKDIR /root/.ssh
COPY id_rsa.pub authorized_keys
RUN chmod 644 -R /root/.ssh

RUN useradd -m ${USER} -u ${UID} -U
# -g ${GID}
RUN groupadd wheel
RUN gpasswd -a ${USER} sudo
RUN gpasswd -a ${USER} wheel
RUN gpasswd -a root wheel

RUN git clone https://github.com/pyenv/pyenv.git /home/dev/.pyenv
RUN echo 'export PYENV_ROOT="$HOME/.pyenv"' >> /home/dev/.bashrc
RUN echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /home/dev/.bashrc
RUN echo 'eval "$(pyenv init --path)"' >> /home/dev/.bashrc
RUN echo 'eval "$(pyenv init -)"' >> /home/dev/.bashrc
RUN chown ${USER}:${USER} -R /home/${USER}/.pyenv
# RUN /root/.pyenv/bin/pyenv install ${FIRST_PYTHON}

RUN echo '%wheel    ALL=(ALL)   NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'Defaults timestamp_timeout = 0' >> /etc/sudoers 

# If you do not need PASSWORD, please comment out below lines ---->
RUN touch /home/dev/.env_passwd
RUN echo 'source /home/dev/.env_passwd' >> /home/${USER}/.bashrc
RUN echo 'if [ -z ${EXISTS_PASSWD} ]; then' >> /home/dev/.bashrc
RUN echo -n 'export LINE=`sudo cat /etc/shadow | grep ' >> /home/${USER}/.bashrc
RUN echo -n "${USER}" >> /home/${USER}/.bashrc
RUN echo '`' >> /home/${USER}/.bashrc
RUN echo 'if [ ` echo \$LINE  | grep "!" ` ]; then' >> /home/${USER}/.bashrc
RUN echo 'echo "register password..."' >>/home/${USER}/.bashrc
RUN echo 'while :; do' >> /home/${USER}/.bashrc
RUN echo "sudo passwd ${USER}" >> /home/${USER}/.bashrc
RUN echo 'if [ $? -eq 0 ];then' >> /home/${USER}/.bashrc
RUN echo '  echo "export EXISTS_PASSWD=1" >> /home/dev/.env_passwd' >> /home/dev/.bashrc
RUN echo '  sudo sed -i -e "s/%wheel    ALL=(ALL)   NOPASSWD:ALL/#%wheel    ALL=(ALL)   NOPASSWD:ALL/" /etc/sudoers' >> /home/${USER}/.bashrc
RUN echo '  break' >> /home/${USER}/.bashrc
RUN echo 'fi' >> /home/${USER}/.bashrc
RUN echo 'echo "Retry... "' >> /home/${USER}/.bashrc
RUN echo 'done' >> /home/${USER}/.bashrc
RUN echo 'fi' >> /home/${USER}/.bashrc
RUN echo 'fi' >> /home/${USER}/.bashrc
RUN chown dev:dev /home/dev/.env_passwd
# ------------> to this point

WORKDIR /root
# sudoを使用できるようにする
# RUN echo "${USER} ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/${USER}
# 一般ユーザがユーザ/グループを追加できるようにする
# entrypoint.sh内でパーミッションを元に戻す
# RUN chmod u+s /usr/sbin/useradd \
#     && chmod u+s /usr/sbin/groupadd
# COPY entrypoint.sh entrypoint.sh
# RUN chmod +x entrypoint.sh
# CMD [ "./entrypoint.sh" ]

USER root
RUN mkdir /home/dev/.ssh
WORKDIR /home/dev/.ssh
COPY id_rsa.pub authorized_keys
RUN chmod 700 -R . && chmod 644 authorized_keys

USER root
RUN chown dev:dev -R /home/dev/.ssh
# USER ${USER}

# RUN ssh-keygen -A

WORKDIR /workspace
EXPOSE 22
# cointainer surely execute below command(s)
ENTRYPOINT ["/usr/sbin/sshd", "-D"]
# ---------------> to this point