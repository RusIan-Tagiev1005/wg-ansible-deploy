


Перед запуском плейбуков необходимо прописать все параметры в `inventory.ini`

Для корректной работы необходима сетевая связность с узлами

# [all:vars]

`listen_port` - порт, который будут слушать все узлы в VPN

`network_mask` - маска сети, см. файлы в `templates`


ex.

listen_port=23122

network_mask=24

# [gateway]

`ansible_host` - ip/доменное имя узла в сетм

`wg_ip` - ip узла в VPN

ex.

node_gw ansible_host=192.168.1.2 wg_ip=10.0.0.1

# [nodes]

`ansible_host` - см. выше

`wg_ip` - см. выше



ex.

node0 ansible_host=192.168.1.201 wg_ip=10.0.0.2

node1 ansible_host=192.168.3.203 wg_ip=10.0.0.3

node2 ansible_host=rogaicopita.su wg_ip=10.0.0.4

node3 ansible_host=node3.lan wg_ip=10.0.0.5

# [gateway:vars]

`ansible_user` - пользователь, от которого будет выполнятся настройка на узле

`interface` - имя интерфейса ноды, через который идет трафик VPN

`gateway_interface` - интерфейс ноды, через который выполняется подключение к ноде


ex.

ansible_user=root

interface=wg0-gw

gateway_interface=eth0

# [nodes:vars]

`ansible_user` - см выше

`interface`- имя интерфейса ноды, через который идет трафик VPN


ex.

interface

ansible_user

## Порядок запуска (из директории репозитория)

1. `ansible-playbook playbooks/all/install_wireguard.yaml`
2. `ansible-playbook playbooks/gateway/configure_wireguard_gateway.yaml`
3. `ansible-playbook playbooks/node/configure_wireguard_node.yaml`
4. `ansible-playbook playbooks/gateway/add_node.yaml`
5. `ansible-playbook playbooks/node/turn_wireguard_on.yaml`


## Про inventory.ini

Хорошим подключатся к нодам не по `root`, а

использовать `ansible` пользователя с соотвествующими правами

Нужно указывать интерфейс шлюза, через который выполняется подключение

из-за postup postdown правил в `wg0-gw.conf`

## Про удаление нод
Удалить ноду из сети можно с помощью ansible-playbook `ansible-playbook playbooks/gateway/remove_node.yaml`

при этом необходимо в `inventory.ini` в секции `[nodes]` оставить только ноду, которую необходимо удалить

## Про добавление нод

Добавить ноду в сеть можно с помощью ansible-playbook `ansible-playbook playbooks/gateway/add_node.yaml`

при этом необходимо в `inventory.ini` в секции `[nodes]` оставить только ноду, которую необходимо добавить







## Прочее

ssh ключи между контроллером и нодами не настраивает

Задаваемый в `inventory.ini` порт `listen_port` не открывает

Удалить wireguard с узлов можно с помощью `ansible-playbook playbooks/all/remove_wireguard.yaml`
`remove_wireguard.yaml`:
- дисейблит и останавливает `wg-quick` сервис
- удаляет пакет wireguard-tools
- удаляет папку /etc/wireguard



# Directory Structure
```
wg-ansible-deploy
      ├── files
      │   ├── logs
      │   │   └── ansible.log
      │   └── templates
      │       ├── wg0-gw.conf
      │       └── wg0-n.conf
      ├── inventory
      │   └── inventory.ini
      ├── playbooks
      │   ├── all
      │   │   ├── install_wireguard.yaml
      │   │   └── remove_wireguard.yaml
      │   ├── gateway
      │   │   ├── add_node.yaml
      │   │   ├── configure_wireguard_gateway.yaml
      │   │   └── remove_node.yaml
      │   └── node
      │       ├── configure_wireguard_node.yaml
      │       └── turn_wireguard_on.yaml
      ├── ansible.cfg
      └── README.md

```
# to-do

- чек пингом все хосты
- сделать открытие портов на узлах
