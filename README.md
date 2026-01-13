
## Порядок запуска (из директории репозитория)

1. `ansible-playbook playbooks/all/install_wireguard.yaml`
2. `ansible-playbook playbooks/gateway/configure_wireguard_gateway.yaml`
3. `ansible-playbook playbooks/node/configure_wireguard_node.yaml`
4. `ansible-playbook playbooks/gateway/add_node.yaml`
5. `ansible-playbook playbooks/node/turn_wireguard_on.yaml`


## Про удаление нод
Удалить ноду из сети можно с помощью ansible-playbook `ansible-playbook playbooks/gateway/remove_node.yaml`

при этом необходимо в `inventory.ini` в секции `[nodes]` оставить только ноду, которую необходимо удалить

## Про добавление нод

Добавить ноду в сеть можно с помощью ansible-playbook `ansible-playbook playbooks/gateway/add_node.yaml`

при этом необходимо в `inventory.ini` в секции `[nodes]` оставить только ноду, которую необходимо добавить





## Про inventory.ini

Хорошим подключатся к нодам не по `root`, а

использовать `ansible` пользователя с соотвествующими правами

Нужно указывать интерфейс шлюза, через который выполняется подключение

из-за postup postdown правил в `wg0-gw.conf`


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
