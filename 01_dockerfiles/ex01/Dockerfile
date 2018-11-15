# docker build -t test01 .
# docker run --name test01 -p 9987:9987/udp -p 10011:10011 -p 30033:30033 -d test01
# Installer et lancer le client TS puis se connect a l'ip de la VM (port 9987 par defaut)
# copier token de la commande docker logs test01 dans TS

FROM debian:latest
LABEL maintainer="bhamdi <bhamdi@student.42.fr>"
RUN apt-get update -y
RUN apt-get install bzip2 wget -y
RUN wget http://dl.4players.de/ts/releases/3.0.13.8/teamspeak3-server_linux_amd64-3.0.13.8.tar.bz2
RUN bunzip2 *.bz2 && tar -xvf *.tar
EXPOSE 9987/udp 30033 10011
Entrypoint ["sh", "teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"]
