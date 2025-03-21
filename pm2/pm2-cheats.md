### install pm2

```
npm install -g pm2
```

### pm2 version

```
pm2 -v
```

### To autostart pm2 on server restart

```
pm2 startup
```

### Check PM2 Processes

```
pm2 list
```

### Save the Process List

```
pm2 save
```

### Logs

```
pm2 logs
```

### Manual Start

```
pm2 start <app-name-or-id>
```

### pm2 config

```json
// ecosystem.config.json

{
  "apps": [
    {
      "name": "app1",
      "script": "./app1.js",
      "instances": "max",
      "exec_mode": "cluster",
      "env": {
        "NODE_ENV": "production",
        "PORT": 3000
      },
      "env_development": {
        "NODE_ENV": "development",
        "PORT": 3001
      }
    },
    {
      "name": "app2",
      "script": "./app2.js",
      "instances": 1,
      "env": {
        "NODE_ENV": "production",
        "PORT": 4000
      }
    }
  ]
}
```

or

```yml
# ecosystem.config.yml
apps:
  - name: app1
    script: ./app1.js
    instances: max
    exec_mode: cluster
    env:
      NODE_ENV: production
      PORT: 3000
    env_development:
      NODE_ENV: development
      PORT: 3001

  - name: app2
    script: ./app2.js
    instances: 1
    env:
      NODE_ENV: production
      PORT: 4000
```

### start apps with config file

```bash
pm2 start ecosystem.config.json
pm2 start ecosystem.config.yml

```

### restart all config apps

```
pm2 reload ecosystem.config.json
```
