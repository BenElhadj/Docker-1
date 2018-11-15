# docker-machine create --driver virtualbox --virtualbox-memory 4096 --virtualbox-cpu-count 2 SuperChar
# docker build -t itest03 .
# Pour le tester lancer en background
# docker run --name gitlab_c -p 443:443 -p 80:80 -p 2222:22 -d itest03
# docker logs -f gitlab_c  (suivre les logs de lancement)
# 
# Dans chrome ouvrir http://192.168.99.100
# default user root, pass 5iveL!fe, changement mdp a la premiere connexion en HTTP
# 
# Connexion SSH
# Generer une cle SSH:  ssh-keygen -t rsa -b 4096 -C "bhamdi@student.42.fr" -f ~/.ssh/gitlab_ssh.key -N "admin"
# Copier la cle dans le clipboard:  pbcopy < ~/.ssh/gitlab_ssh.key.pub
# Dans l'interface web ajoute la cle public et sauvegarder
# Faire: eval "$(ssh-agent -s)"
# Ensuite: ssh-add -K ~/.ssh/gitlab_ssh.key (mot de passe "admin")
# git clone ssh://git@192.168.99.100:2222/root/test-gitlab.git test_ssh

FROM ubuntu:14.04
LABEL maintainer="bhamdi <bhamdi@student.42.fr>"

# Install les packages
RUN apt-get update -q \
&& DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
ca-certificates \
openssh-server \
wget \
apt-transport-https \
vim \
sudo \
curl 

# Install GitLab derniere version stable
RUN echo "deb https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/gitlab_gitlab-ce.list
RUN wget -q -O - https://packages.gitlab.com/gpg.key | apt-key add -
RUN apt-get update && apt-get install -yq --no-install-recommends gitlab-ce

# Manage SSHD through runit
RUN mkdir -p /opt/gitlab/sv/sshd/supervise \
&& mkfifo /opt/gitlab/sv/sshd/supervise/ok \
&& printf "#!/bin/sh\nexec 2>&1\numask 077\nexec /usr/sbin/sshd -D" > /opt/gitlab/sv/sshd/run \
&& chmod a+x /opt/gitlab/sv/sshd/run \
&& ln -s /opt/gitlab/sv/sshd /opt/gitlab/service \
&& mkdir -p /var/run/sshd

# Disabling use DNS in ssh since it tends to slow connecting
RUN echo "UseDNS no" >> /etc/ssh/sshd_config

# Prepare default configuration
RUN ( \
echo "" && \
echo "# Docker options" && \
echo "# Prevent Postgres from trying to allocate 25% of total memory" && \
echo "postgresql['shared_buffers'] = '1MB'" ) >> /etc/gitlab/gitlab.rb && \
mkdir -p /assets/ && \
cp /etc/gitlab/gitlab.rb /assets/gitlab.rb

# Expose web & ssh
# 443 -> HTTPS, 22 -> SSH
EXPOSE 443 80 22

# Define data volumes (si on souhaite conserver les data on peut bind les volumes)
VOLUME ["/etc/gitlab", "/var/opt/gitlab", "/var/log/gitlab"]

# Create wrapper entrypoint (version simple)
RUN (printf '#!/bin/bash\n\nset -e\n\n\
function sigterm_handler() {\n\techo "SIGTERM signal received, try to gracefully shutdown all services..."\n\tgitlab-ctl stop\n}\n\ntrap "sigterm_handler; exit" TERM\n\n/opt/gitlab/embedded/bin/runsvdir-start &\n\ngitlab-ctl reconfigure\n\ngitlab-ctl tail\n\n') \
> /usr/local/bin/wrapper && chmod +x /usr/local/bin/wrapper

# Wrapper to handle signal, start service manager and reconfigure GitLab
ENTRYPOINT ["/usr/local/bin/wrapper"]

