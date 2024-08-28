# Evaluation von **outline** für pme

- [Evaluation von **outline** für pme](#evaluation-von-outline-für-pme)
  - [Konfiguration](#konfiguration)
  - [Lokale docker-compose Umgebung](#lokale-docker-compose-umgebung)
  - [Schritte für das Deployment](#schritte-für-das-deployment)
    - [Voraussetzungen:](#voraussetzungen)
    - [Anleitung](#anleitung)
    - [Portainer-Besonderheit](#portainer-besonderheit)


## Konfiguration

ein Beispiel findest du hier [stack.EXAMPLE.env](./stack.EXAMPLE.env)

Variable|Wert|Beschreibung
-|-|-
HTTP_PORT_MAP|'13000:3000'|
REDIS_PORT_MAP|'6379:6379'|
POSTGRES_PORT_MAP|'5432:5432'|
POSTGRES_USER|'user'|
POSTGRES_PASSWORD|'pass'|
POSTGRES_DB|'outline'|
NODE_ENV|production|
SECRET_KEY|_key_|
UTILS_SECRET||
DATABASE_URL=|postgres://user:pass@postgres:5432/outline||
PGSSLMODE|disable|
REDIS_URL|redis://redis:6379|
URL|https://docs.zameit.com|
PORT|3000|
FILE_STORAGE|local|
FILE_STORAGE_LOCAL_ROOT_DIR|/var/lib/outline/data|
FILE_STORAGE_UPLOAD_MAX_SIZE|262144000|
GOOGLE_CLIENT_ID||
GOOGLE_CLIENT_SECRET||

## Lokale docker-compose Umgebung

```shell
docker-compose --env-file ./stack.env up # we emulate portainers .env behaviour to allow variable substitution
```

## Schritte für das Deployment 

### Voraussetzungen:
- Docker-Host
- Portainer


### Anleitung

Wir loggen uns auf der Portainer ein und fügen einen neuen **Stack** hinzu. Die Stack-Definition besteht aus 
zwei Komponenten:
- Docker Compose file (`docker-compose.yml`)
- Umgebungsvariablen (`stack.env`)

_In dem Repository kann die `stack.env` nicht liegen, da sich in ihr Secrets und Passwörter befinden und dieses Repo hier 
public ist und es generell keine gute Praxis ist, diese mit zu committen._ 

_Allerdings liegt eine (bereinigte) Vorlage für `docs.zameit.com` bei (`stack.EXAMPLE.env`)._

- Wir legen einen Stack an und vergeben einen Namen
- Kopieren das docker compose file und die stack.env von Hand rein oder laden die Umgebung via Repository (Punkt 2 und 3).
- Dann kümmern wir uns um die Umgebungsvariablen, die wir per Datei hochladen oder reinkopieren (Punkt 4.)
  

### Portainer-Besonderheit
Der Name `stack.env` in der `env_file` Direktive führt **automatisch** dazu, dass die Umgebungsvariablen die in Portainer definiert werden, dem jeweiligen Container bekannt gemacht werden.

Lässt man diese weg, muss man händisch die docker-compose `environment` Syntax für jede Variable und jeden Container gebrauchen.:

```yaml
services:
  container:
    environment:
      variable: {variable}
```

![Create a new stack](docs/portainer_add_new_stack-1.png)
![Configuring the stack](docs/portainer_add_new_stack-2.png)

