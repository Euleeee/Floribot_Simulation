# Floribot_Simulation

In diesem Git liegen die Dateien für die Floribot Simulation ab.
Um das Repository zu klonen folgenden Befehl benutzen:

    git clone https://github.com/Euleeee/Floribot_Simulation catkin_ws

Existiert der Ordner bereits, muss er zuerst gelöscht werden. Optional könnte man auch das <catkin_ws> durch einen anderen Zielordnernamen ersetzen.

# Benötigte Software
ROS Noetic: http://wiki.ros.org/noetic/Installation/Ubuntu <br>
ROS Velocity Controllers: 

    sudo apt install ros-noetic-velocity-controllers
    
Gazebo 11: 

    curl -sSL http://get.gazebosim.org | sh
    
Python: 

    sudo apt install python-is-python3
    
XACRO:

    sudo apt install ros-neotic-xacro
    
Anmerkung XACRO: Nach dem sudo apt install Befehl die xacro.py aus dem Git herunterladen und überpüfen, ob das Skript als Programm ausgeführt werden kann. Dazu Rechtsklick auf das xacro.py -> Dann auf Eigenschaften -> Dann im Reiter auf Zugriffsrechte ->  Überprüfen, ob im Punkt "Ausführen:" die Checkbox ein Häkchen hat. Ist das der Fall, dann per folgenden Befehl die Datei in den Zielordner kopieren.

    sudo cp <Downloadort> /opt/ros/noetic/share/xacro 

# Installation Realsense Dependencies

-	Zunächst sudo apt-get install ros-noetic-ddynamic-reconfigure ausführen
-	Danach die Installationsanleitung unter https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md befolgen, wichtig auch librealsense2-dev Paket installieren

# Starten der Simulation
In dieser Anleitung wird davon ausgegangen, dass der Catkin Workspace im Ordner catkin_ws abliegt. <br>
Ein neues Terminal öffnen und folgenden Befehl eingeben:

    cd ~/catkin_ws && catkin_make && source ~/.bashrc 

Nachdem der Befehl eingegeben wurde, wirde der Catkin Workspace über catkin_make erstellt und es erscheinen im Ordner catkin_ws zwei neue Unterordner: "build" und "devel". <br>
Anschließend kann die Simulation mit dem folgendem Befehel gestartet werdeb:

    roslaunch floribot_simulation FloriBot.launch 

Sollte eine Fehlermeldung kommen, folgenden Befehl eingeben und anschließend erneut den roslaunch.. Befehl ausführen. (Zu Beachten: Euren Nutzernamen ohne die eckigen Klammern einsetzen)

    source  /home/<nutzername>/catkin_ws/devel/setup.bash 
    
Sobald die Simulation läuft, kann mit dem Roboter mit folgendem Befehl eine Geschwindigkeit vorgegeben werden, dazu muss jedoch ein neues Terminal geöffnet werden: 

    rostopic pub /cmd_vel geometry_msgs/Twist -r 10 -- '[0.2, 0.0, 0.0]' '[0.0, 0.0, 0.0]' 
    
Die Simulation kann mit der Tastenkombination Strg+C beendet werden. 

# Docker Umgebung
Für die Docker Umgebung kann sich an den FRE Repo's orientiert werden. Um ein Docker Image vom Floribot zu erstellen, muss dich sich das Docker-File im Worksapce befinden und dann folgender Befehl ausgeführt werden (nähere Infos dazu: https://github.com/FieldRobotEvent/example_ws)

    docker build . -t robot_workspace

Neben dem Roboterimage muss noch ein Simulationsimage erstellt werden, welches mit dem Competition Enviornment verkünpft ist. Dazu muss zuerst das Git Repository des Competion Enviornments geklont werden.

    git clone https://github.com/FieldRobotEvent/competition_environment.git
    
Anschließend werden die benötigten Simulationsdatei aus dem Roboter Image für das Simulations Image kopiert mit dem Befehl. Um das Skript auszuführen, muss man sich im Ordner des geklonten Competition Enviornemnt befinden.

    python3 scripts/copy_simulation_files.py
    
Nach dem das Kopieren abgeschlossen wurde, muss man nun in den Unterordner task_navigation gehen, um die Navigationsumgebung zu laden. Wenn man im Ordner ist, kann man mit

    docker-compose up
    
die Simulation über Docker starten. Über http://localhost:8080/ kann man sich die Simulation anzeigen lassen. Die verschiedenen Topics kann man sich an seinem lokalen Rechner wie folgt anzeigen lassen. Zuerst muss man ein neues Terminal öffnen und anschließend 

    ROS_MASTER_URI=http://172.20.0.5:11311
    
eingeben. Damit wird der ROS Master auf eine andere Andresse gelegt. Nun können alle Topics mit dem Befehl 'rostopic list' angezeigt werden. Schließt man das Terminal, so wird die ROS Master Adresse wieder auf die default Adresse zurückgesetzt. Um die Simulation zu beenden, einfach per Strg+c die Simulation abbrechen und anschließend den Befehl

    docker-compose down
    
benutzen, um die Simulation vollständig zu beenden. 

# Aktualisieren der Docker Umgebung
Um den Docker Simulationscontainer zu aktualisieren fogledenn Befehl ausführen: 

    docker pull fieldrobotevent/simulation:latest
