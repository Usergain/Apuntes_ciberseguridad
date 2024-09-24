```
 1  ls
    2  sudo apt-get update
    3  sudo apt-get install ca-certificates curl gnupg lsb-release
    4  sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    5  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    6  sudo add-apt-repository 'deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable'
    7  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    8  sudo systemctl start docker
    9  sudo systemctl stop docker
   10  sudo stop docker
   11  stop docker
   12  sudo systemctl stop docker
   13  sudo systemctl start docker
   14  ls
   15  mkdir hacking
   16  ls
   17  cd
   18  ls
   19  cd hacking/
   20  ls
   21  touch docker-compose.yml
   22  nano docker-compose.yml
   23  docker compose up -d
   24  sudo docker compose up -d
   25  docker compose start
   26  sudo docker compose start
   27  ls
   28  nano docker-compose.yml
   29  docker compose stop
   30  sudo docker compose stop
   31  clear
   32  docker compose rm
   33  sudo docker compose rm
   34  sudo compose up -d
   35  sudo docker compose up -d
   36  docker ps
   37  sudo docker ps
   38  docker stop
   39  sudo docker ps
   40  docker compose stop
   41  sudo docker compose stop
   42  sudo docker stop
   43  sudo docker stop 6210106140be
   44  docker ps
   45  sudo docker ps
   46  history
   k4rma@Arkaitxiki:~$ sudo docker start 6210106140be
6210106140be
k4rma@Arkaitxiki:~$ sudo docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS          PORTS                                   NAMES
6210106140be   greenbone/gsa:stable             "/usr/local/bin/entr…"   44 minutes ago   Up 5 seconds    0.0.0.0:9392->80/tcp, :::9392->80/tcp   hacking-gsa-1
eab80fdcc052   greenbone/gvmd:stable            "/usr/local/bin/entr…"   44 minutes ago   Up 44 minutes                                           hacking-gvmd-1
c4954d252c70   greenbone/ospd-openvas:stable    "/usr/bin/tini -- /u…"   44 minutes ago   Up 43 minutes                                           hacking-ospd-openvas-1
ac5494b5758f   greenbone/notus-scanner:stable   "/usr/local/bin/entr…"   44 minutes ago   Up 44 minutes                                           hacking-notus-scanner-1
f302f0fd0e70   greenbone/redis-server           "/bin/sh -c 'rm -f /…"   44 minutes ago   Up 44 minutes                                           hacking-redis-server-1
c8442e2ed42b   greenbone/mqtt-broker            "/bin/sh -c 'mosquit…"   44 minutes ago   Up 44 minutes                                           hacking-mqtt-broker-1
2a62dad8af4e   greenbone/pg-gvm:stable          "/usr/local/bin/entr…"   44 minutes ago   Up 44 minutes                                           hacking-pg-gvm-1
```

![[Pasted image 20240714041516.png]]
Para sacar un archivo del contenedor al directorio actual:
`docker cp {nombre_container}:{path_completo_archivo} {directorio_destino}` 
`chsh -s /usr/bin/zsh` `sudo usermod --shell /usr/bin/zsh root` `sudo ln -s -fv ~/.zshrc /root/.zshrc`

admin -> admin

![[Pasted image 20240714042531.png]]

