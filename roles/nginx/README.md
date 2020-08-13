Nginx
=========

Роль Ansible для установки и простой настройки Nginx

Переменные
--------------

`/vars/main.yml`:

```yml
template_conf_file: "путь к файлу шаблона конфига nginx.conf"
template_vhost_file: "путь к файлу шаблона конфига виртуальных хостов"
template_demo_html_file: "путь к файлу шаблона демонстрационного HTML-файла"
create_demo_html: "создавать демонстрационный HTML-файл? true или false"
```

`/vars/{Debian,RedHat}.yml`:

Здесь задаются переменные различающиеся в зависимости от дистрибутива

```yml
nginx_user: "системный пользователь Nginx"
```

`/defaults/main.yml`:

Здесь задаются переменные для настройки конфига nginx.conf и конфигов виртуальных хостов. 

В `vhosts` определяется из чего будет состоять секция `server`. `extra` позволяет задать location или что-нибудь подобное, используемое внутри секции `server`. `root` определяет где будут храниться файлы виртуального хоста и если этой директории нет - она будет создана. `filename` определяет имя файла хоста в `/etc/nginx/conf.d`

```yml
# nginx.conf
workers: "1024"
gzip_state: "on"
tcp_nopush: "on"
tcp_nodelay: "on"

# Virtual Hosts
vhosts:
  - listen: "8080"
    server_name: "vhost1"
    root: "/var/www/html/vhost1"
    filename: "vhost1.conf"
    extra: |
      location / {
        try_files $uri $uri/ =404;
      }
  - listen: "8081"
    server_name: "vhost2"
    root: "/var/www/html/vhost2"
    filename: "vhost2.conf"
    extra: |
      location / {
        try_files $uri $uri/ =404;
      }
```

Пример Playbook
----------------

```yml
- hosts: all
  roles:
      - nginx
```

```
$ ansible-playbook playbook.yml
```