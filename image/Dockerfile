FROM python:3.7.3-slim-stretch

RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
  sudo git ca-certificates wget \
  build-essential && \
  git clone --depth 1 -b 0.6.2 https://github.com/ai-traders/dojo.git /tmp/dojo_git &&\
  /tmp/dojo_git/image_scripts/src/install.sh && \
  rm -r /tmp/dojo_git && \
  echo 'dojo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

RUN pip install virtualenv tox

# ensure pretty bash prompt
COPY bashrc /home/dojo/.bashrc
COPY profile /home/dojo/.profile
RUN chown dojo:dojo -R /home/dojo

COPY etc_dojo.d/scripts/* /etc/dojo.d/scripts/

RUN sudo -u dojo pip install --user pipenv

ENTRYPOINT ["/usr/bin/entrypoint.sh"]
CMD ["/bin/bash"]
