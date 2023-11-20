# Systemd

![pic](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Za_VipsuGjbjm-xN3UMfTw.gif)

## Настройка сервиса NodeJS

1. Создаем файл с названием `название_сервиса.service` в `/etc/systemd/system/`
2. `nano название_сервиса.service`, заполняем конфиг файл

```ini
  [Unit]
  Description=My Little Server
  # Documentation=https://
  # Author: Natan Cabral
  [Service]
  # Start Service and Examples
  # Путь до бинарника node и путь до запускаемого файла
  ExecStart=/usr/local/bin/node /home/myserver/server.js
  # ExecStart=/usr/bin/sudo /usr/bin/node /home/myserver/server.js
  # ExecStart=/usr/local/bin/node /var/www/project/myserver/server.js
  # Options Stop and Restart
  # ExecStop=
  # ExecReload=
  # Required on some systems
  # WorkingDirectory=/home/myserver/
  # WorkingDirectory=/var/www/myproject/
  # Restart service after 10 seconds if node service crashes
  RestartSec=10
  Restart=always
  # Restart=on-failure
  # Output to syslog
  StandardOutput=syslog
  StandardError=syslog
  SyslogIdentifier=nodejs-my-server-example
  # #### please, not root users
  # RHEL/Fedora uses 'nobody'
  # User=nouser
  # Debian/Ubuntu uses 'nogroup', RHEL/Fedora uses 'nobody'
  # Group=nogroup
  # variables
  Environment=PATH=/usr/bin:/usr/local/bin
  # Environment=NODE_ENV=production
  # Environment=NODE_PORT=3001
  # Environment="SECRET=pGNqduRFkB4K9C2vijOmUDa2kPtUhArN"
  # Environment="ANOTHER_SECRET=JP8YLOc2bsNlrGuD6LVTq7L36obpjzxd"
```

3. Запускаем сервис `systemctl start название_сервиса.service`
4. Проверяем статус `systemctl status название_сервиса.service`
5. Подключаемся к журналу логов `journalctl -u название_сервиса.service -f`.
   - `-f` - отслеживать в реальном времени
6. Команды для работы с сервисом

```bash
# Перезагрузка всех сервисов
systemctl daemon-reload

# Перезагрузка конкретного сервиса
systemctl restart название_сервиса.service

# Разрешить сервис
systemctl enable название_сервиса.service

# Список запущенных сервисов
ps -ef | grep файл_который_мы_запустили.js

# Мягко убить процесс
kill -9 {numberPID}
```

## Запуск сервиса NodeJS + package.json (TypeScript)

От предыдущего варианта отличается только конфиг файл. Команды для работы с сервисом буду прежними.

Содержимое `название_сервиса.service`:

```ini
[Unit]
Description=description
[Service]
# ExecStart=путь_до_ноды путь_до_yarn команда_из_package.json
ExecStart=/root/node/bin/node /root/node/bin/.yarn/yarn start

# путь до папки проекта:
WorkingDirectory=/root/app/my-app/

# Если используем type:module раскомментировать след строчку:
# Environment=NODE_OPTIONS='--experimental-import-meta-resolve'

StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=dollar-tracker-v3
```

Содержимое `package.json`:

```json
{
  "scripts": {
    "start": "yarn ts-node src/index.ts"
  }
}
```

## Статья на medium.com

[ссылка на статью](https://natancabral.medium.com/run-node-js-service-with-systemd-on-linux-42cfdf0ad7b2)

[Подробное описание всех пунктов конфиг файла](https://habr.com/ru/companies/slurm/articles/255845/)
