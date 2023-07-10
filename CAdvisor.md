Mit dem untenstehenden Docker Compose-File wird ein Container mit "Google CAdvisor" gestartet, der über Port 8080 erreichbar ist.

Das Image wird vom Docker Hub (google/cadvisor) bezogen.
Weitere Informationen zu CAdvisor finden Sie hier.
Der Container-Name wurde als "cadvisor" festgelegt.
Der Container nutzt den Port 8080.
Es werden entsprechende Volumes erstellt.

Das File:
version: '3'

services:
cadvisor:
image: google/cadvisor
container_name: cadvisor
restart: always
ports:
- "8080:8080"
volumes:
- "/:/rootfs:ro"
- "/var/run:/var/run:rw"
- "/sys:/sys:ro"
- "/var/lib/docker/:/var/lib/docker:ro"
