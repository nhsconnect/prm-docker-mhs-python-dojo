FROM python:3.7.3-slim-stretch

RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
  sudo git ca-certificates wget \
  build-essential && \
  git clone --depth 1 -b 0.6.2 https://github.com/kudulab/dojo.git /tmp/dojo_git &&\
  /tmp/dojo_git/image_scripts/src/install.sh && \
  rm -r /tmp/dojo_git && \
  echo 'dojo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

RUN apt-get update && \
  apt-get install -y build-essential libssl-dev swig pkg-config

# Temporarily lower security to workaround opentest certs with SHA1 signatures
RUN sed -i 's/SECLEVEL=2/SECLEVEL=1/' /etc/ssl/openssl.cnf

RUN pip install virtualenv tox \
  unittest-xml-reporting \
  awscli \
  python-qpid-proton

# Install jq to parse json in bash
RUN wget -O ./jq-linux64 https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 &&\
  chmod +x ./jq-linux64 &&\
  mv ./jq-linux64 /usr/bin/jq

# ensure pretty bash prompt
COPY bashrc /home/dojo/.bashrc
COPY profile /home/dojo/.profile
RUN chown dojo:dojo -R /home/dojo

COPY etc_dojo.d/scripts/* /etc/dojo.d/scripts/

RUN sudo -u dojo pip install --user pipenv

ENTRYPOINT ["/usr/bin/entrypoint.sh"]
CMD ["/bin/bash"]
