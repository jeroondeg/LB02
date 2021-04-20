# M300-Services-LB02

## 30 Container
### Was versteht man unter Containisierung?
Bei der Containerisierung handelt es sich um eine Art Virtualisierung auf Anwendungsebene, bei der mehrere isolierte Userspace-Instanzen auf einem einzelnen Kernel ausgeführt werden können. Diese Instanzen werden Container genannt.

### Welche Vorteile bringt es mit sich?
#### Resourcenbedarf 
* Container benötigen auf dem Server weniger Ressourcen als virtuelle Maschinen und sind normalerweise innerhalb weniger Sekunden gestartet.
#### Elastizität
 * Container sind hochelastisch und müssen nicht mit einer bestimmten Menge an Ressourcen ausgestattet werden. Dies bedeutet, dass Container die Ressourcen des Servers effizienter und dynamischer nutzen können.
#### Performance 
* Bei hohen, konkurrierenden Ressourcenanforderungen ist die Leistung von Anwendungen  die aus einer Containerumgebung ausgeführt werden weitaus besser als bei der Ausführung in einer virtuellen Maschine.


### 01 Docker 
Docker ist eine Freie Software zur Isolierung von Anwendungen mit Hilfe von Containervirtualisierung. Docker vereinfacht die Bereitstellung von Anwendungen, weil sich Container, die alle nötigen Pakete enthalten, leicht als Dateien transportieren und installieren lassen. Container gewährleisten die Trennung und Verwaltung der auf einem Rechner genutzten Ressourcen. 


### 02 Virtuelle Maschine vs Docker

Wie man gut erkennen kann, geht es hier um den Vergleich zwischen virtuellen Maschinen und Containern. Beide haben ihre Vor und Nachteile. Doch für die minimierung der weniger wichtigen Ressourcen ist Docker sehr gut geeignet. In diesem Beispiel hat auf der virtuellen Maschine Seite jede App ein eigenes Betriebssystem. Dies ist bei Containiesierung (Docker) nicht der Fall. Da wird das Betriebssystem von unserem Dockerbereitgestellt und die Apps teilen sich dies.

### 03 Webserver einrichten
Zuerst muss das Image erstellt werden. Mit docker build.

![alt text](Bilder/6.JPG "VMvsDocker")


Anschliessend wird der Port 5050 konfiguriert für den Webserver.

![alt text](Bilder/4.JPG "PortConfig")

Nun kann man mit docker ps überprüfen ob dieses Image iene Container ID bekommen hat und ob der Port eingetragen ist.

![alt text](Bilder/8.JPG "Check")

Zum Schluss kann man mit der eigenen IP und dem richtigen Port überprüfen ob der Webserver erreichbar ist. 

![alt text](Bilder/7.JPG "Check")



### 04 Docker Befehle
| Befehl            | Funktion                                             |
| -------------     | ---------------------------------------------------- | 
| ```docker pull```     | Holt ein Image. |
| ```docker run```      | Started VM mit dem ausgewähltem Image. |
| ```docker ps```       | Zeigt laufende Maschinen. |
| ```docker version```  | Zeigt die Docker Version von Echo-Client und Server an. |
| ```docker images```   | Listet alle Docker Images auf. |
| ```docker exec```     | Führt einen Befehl in einem laufenden Container aus. |
| ```docker search```   | Durchsucht das Docker Hub nach Images. |
| ```docker attach```   | Hängt etwas an einen laufenden Container an. |
| ```docker commit```   | Erstellt ein neues Image mit den Änderungen, die an einem Container vorgenommen worden sind. |
| ```docker stop```     | Haltet die gewünschte Maschine an. |

### 05 Netzwerkplan

![alt text](Bilder/10.JPG "VMvsDocker")


## 35 Sicherheit

### 01 Monitoring 
Nun geht es darum das ein Monitoring eingerichtet wird. Hierfür verwende ich den Port 8080. Das Image wird gefunden und eine Container ID wird dementsprechend verpasst. 

![alt text](Bilder/2.JPG "Check")

Nun kann man wieder überprüfen ob die Container ID und der Por eingetrgaen wurden unter docker ps.

![alt text](Bilder/1.JPG "Check")

Zum Schluss kann man dann mit dem ausgewählte Port bei mir wäre dies jetzt 8080, auf die Seite via Browser zugegriffen werden. 

![alt text](Bilder/9.JPG "Check")

### 02 Logging

Die Logs können über den Befehl docker logs abgerufen werden. Es gibt mehrere Werte, die man über das Argument von docker auswählen kann:

* json-file 

Ausgaben abholen:
````
$ docker run --name logtest ubuntu bash -c 'echo "stdout"; echo "stderr" >>2'
$ docker logs logtest
$ docker rm logtest
````
Laufende Ausgaben:
````
$ docker run -d --name streamtest ubuntu bash -c 'while true; do echo "tick"; sleep 1; done;'
$ docker logs streamtest
$ docker logs streamtest | wc -l
$ docker rm streamtest
````

Protokollierung in das System-Log des Hosts:
````
$ docker run -d --log-driver=syslog ubuntu bash -c 'i=0; while true; do i=$((i+1)); echo "docker $i"; sleep 1; done;'
$ tail -f /var/log/syslog
````

### 03 Weitere Sicherheitstipps

Hier sind noch weitere Sicherheitstipps welche einem bei einer Verbesserung oder Verschäfung der Sicherheit helfen könnten.

* Netzwerkzugriff beschränken
* setuid/setgid-Binaries entfernen
* Speicher begrenzen

````
$ docker run -m 128m --memory-swap 128m amouat/stress stress --vm 1 --vm-bytes 127m -t 5s
````

* CPU beschränken
````
$ docker run -d --name load3 -c 512 amouat/stress
````
* Neustarts begrenzen
````
$ docker run -d --restart=on-failure:10 my-flaky-image
````
* Zugriffe aus Dateisysteme begrenzen
````
$ docker run --read-only ubuntu touch x
````
* Capabilities einschränken
````
$ docker run --cap-drop all --cap-add CHOWN ubuntu chown 100 /tmp
````
* Ressourcenbeschränkungen anwenden
````
$ docker run --ulimit cpu=12:14 amouat/stress stress --cpu 1
````



## 40 Kubernetes (k8s)

## 80 Ergänzungen zu den Unterlagen

### Vergleich Vorwissen - Wissenszuwachs

Ich hatte vor dem Modul noch fast gar kein Wissen über Docker oder Containerisierung. Nun kann ich aber bereits kleine Services bereitstellen, da mich das Thema jedoch unheimlich interessiert, werde ich am Ball bleiben und noch mehr versuchen dazu zu lernen. Im Geschäft habe ich eine Testumgebung bei der ich demnächst 3 Linux Maschinen über Ansible ausetzen werden und diese dann über Docker betrieben werde.

### Reflexion

Ich finde dass ich durchaus behaupten kann, dass ich etwas unter dem Begriff Container nun verstehe, nun muss ich jedoch nur noch lernen, das gelernte geschickt einzusetzen.