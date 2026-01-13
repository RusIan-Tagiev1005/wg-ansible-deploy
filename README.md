
## Порядок запуска (из директории репозитория)

1. `ansible-playbook playbooks/all/install_wireguard.yaml`
2. `ansible-playbook playbooks/gateway/configure_wireguard_gateway.yaml`
3. `ansible-playbook playbooks/node/configure_wireguard_node.yaml`
4. `ansible-playbook playbooks/gateway/add_node.yaml` *(после добавления ноды перезапускает сервис wg-quick)*
5. `ansible-playbook playbooks/node/turn_wireguard_on.yaml`
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
