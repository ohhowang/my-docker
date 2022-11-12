FROM archlinux:base-devel

WORKDIR /tmp
ENV SHELL /bin/bash
ADD mirrorlist /etc/pacman.d/mirrorlist
ENV UPDATE_TIME 20221112T00:00:00+08:00
RUN yes | pacman -Syu
RUN yes | pacman -S git zsh openssh
RUN mkdir -p /root/.config
VOLUME ["/root/.config", "/root/workspaces"]

# zsh
RUN zsh -c 'git clone https://code.aliyun.com/412244196/prezto.git "$HOME/.zprezto"' &&\
	  zsh -c 'setopt EXTENDED_GLOB' &&\
	  zsh -c 'for rcfile in "$HOME"/.zprezto/runcoms/z*; do ln -s "$rcfile" "$HOME/.${rcfile:t}"; done'
ENV SHELL /bin/zsh
# end

# nvm
ENV NVM_DIR /root/.nvm
ENV NODE_VERSION 18.12.1
ADD nvm-0.39.2 /root/.nvm/
RUN sh ${NVM_DIR}/nvm.sh &&\
	echo '' >> /root/.zshrc &&\
	echo 'export NVM_DIR="$HOME/.nvm"' >> /root/.zshrc &&\
	echo '[ -s "${NVM_DIR}/nvm.sh" ] && { source "${NVM_DIR}/nvm.sh" }' >> /root/.zshrc &&\
	echo '[ -s "${NVM_DIR}/bash_completion" ] && { source "${NVM_DIR}/bash_completion" } ' >> /root/.zshrc
RUN source ${NVM_DIR}/nvm.sh &&\
  nvm install ${NODE_VERSION} &&\
  nvm alias default ${NODE_VERSION} &&\
  nvm use default &&\
  npm i -g yarn pnpm
# end