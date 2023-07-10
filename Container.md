Arbeitsumgebung

Docker Dekstop installiert:
![image](https://github.com/Sildchoedeleti/Modul300Nicolas/assets/133661373/2cc6003d-1cf5-4230-a0c8-41949b025745)


Docker Container anlegen: 
Anlegen eines neuen Verzeichnisses für die jeweiligen Projekte.
Anlegen einer neuen Datei namens Dockerfile, in der der jeweilige Code dokumentiert wird.
Öffnen des jeweiligen Verzeichnisses in der PowerShell. Diese Sitzung dient als Command-Line-Interface (CLI)-Umgebung für den Zugriff.
Nach dem Speichern des fertigen Docker-Images führen Sie den Build unter einem personalisierten Namen durch:
Um den Build zu initiieren, führen Sie "docker build -t webserver ." basierend auf dem Docker-Image im aktuellen Verzeichnis aus.
Um den Docker-Container zu starten und Port 80 des Containers mit Port 8080 des Hosts zu verbinden, führen Sie "docker run -d -p 8080:80 --name webserver webserver" aus.


Docker Befehle, welche sich als Nützlich erweisen könnten:
Befehl	Beschreibung
docker build	Erstellt ein Docker-Image aus einem Dockerfile
docker run	Startet einen Container aus einem Image
docker stop	Stoppt einen laufenden Container
docker start	Startet einen gestoppten Container
docker restart	Startet einen Container neu
docker exec	Führt einen Befehl in einem laufenden Container aus
docker ps	Zeigt Informationen über laufende Container an
docker images	Zeigt eine Liste der verfügbaren Images an
docker pull	Lädt ein Image aus einem Registry-Repository herunter
docker push	Lädt ein Image zu einem Registry-Repository hoch
docker rm	Löscht einen gestoppten Container
docker rmi	Löscht ein Image
docker logs	Zeigt die Logs eines Containers an
docker network	Verwaltet Docker-Netzwerke
docker volume	Verwaltet Docker-Volumes
docker inspect	Zeigt detaillierte Informationen zu einem Container oder Image an
docker-compose up	Startet Container gemäß den Anweisungen in einer docker-compose.yml-Datei
