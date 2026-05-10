### What is?
Automatically restart applications that are down accross various VPCs and get notifications.

### Requirements
- yq (go)
- curl
- ssh

### Setup

```
.
├── configs
│   ├── apps.yaml
│   └── servers.yaml
├── keys
│   └── server1.key
└── vctl
```

- Enter server details in config/servers.yaml. Optional: user & key
  > servers and keys setup is optional if it is alredy configred in system's `~/.ssh/config`
```
- name: sname
  ip: x.x.x.x
  user: ubuntu
  key: server1.key
```

- Put servers' ssh keys in keys/ directory and use the filename in servers config above.

- Enter the apps details in config/apps.yml. Optionl: user & path
```
- name: app_name
  server: sname
  health_check: https://api.app.com
  user: deploy
  path: /var/www/myapp
  command: docker compose up -d --no-deps myapp
```

- set discord webhook in `vctl` file for down and recovery notifications.

### Usage
- `./vctl list`: see list of servers and applications
- `./vctl check`: check status of all the apps and restart if down.
- `./vctl check app_name`: check only specific app
- `./vctl check server_name`: check all apps in a server

### Tips
- To add a public key to authorized keys of multiple servers at once, setup pssh and then:
`pssh -h ~/.pssh/yag -i 'echo "<public_key_here>" | sudo tee -a /home/deploy/.ssh/authorized_keys > /dev/null'`

