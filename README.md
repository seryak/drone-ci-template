### Плагины

Телеграмм сообщения https://plugins.drone.io/appleboy/drone-telegram/
GitHub Pages https://plugins.drone.io/drone-plugins/drone-gh-pages/ 
Download https://plugins.drone.io/drone-plugins/drone-download/ 
Github Releases https://plugins.drone.io/drone-plugins/drone-github-release/
Rsync https://plugins.drone.io/drillster/drone-rsync/
Плагин для сборки образов Docker https://plugins.drone.io/drone-plugins/drone-docker/


Пример pipeline

```yml
kind: pipeline  
type: docker  
name: default  
  
steps:  
- name: 'Устанавливаем зависимости Composer'  
 image: seryak/p80  
  commands:  
    - composer install  
  
- name: 'Копируем файлы в папку релиза'  
 image: alpine  
  volumes:  
    - name: deploy  
      path: /home/deploy  
  commands:  
    - cp -r $PWD/* /home/deploy  
  
volumes:  
  - name: deploy  
    host:  
      path: /home/seryak/drotest  
  - name: docker_socket  
    host:  
      path: /var/run/docker.sock  
  
  
  
  
---  
kind: pipeline  
type: exec  
name: deploy  
#  
platform:  
  os: linux  
  arch: amd64  
  
clone:  
  disable: true  
  
steps:  
  - name: 'Запускаем релиз в докере'  
 commands:  
      - cd /home/seryak/drotest && docker-compose stop  
      - cd /home/seryak/drotest && docker-compose up -d  
  
depends_on:  
  - default
```

