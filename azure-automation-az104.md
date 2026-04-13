[Azure](https://github.com/magnum31415/wiki/blob/main/azure.md)

# Automation

## Automation Account

**Automation Account**:  Es el contenedor / plataforma de ejecución “el runtime donde viven y se ejecutan los runbooks”

Contiene:
- Runbooks
- Schedules
- JobSchedules
- Jobs (histórico de ejecuciones)
- Assets (variables, credentials, etc.)

👉 Sin Automation Account no hay nada


````
Automation Account
   ├── Runbook (código)
   ├── Schedule (tiempo)
   ├── JobSchedule (link)
   │       └── conecta Runbook + Schedule
   └── Jobs (ejecuciones reales)
````

| Concepto           | Analogía           |
| ------------------ | ------------------ |
| Automation Account | Servidor           |
| Runbook            | Script             |
| Schedule           | Cron               |
| JobSchedule        | crontab entry      |
| Job                | ejecución del cron |
