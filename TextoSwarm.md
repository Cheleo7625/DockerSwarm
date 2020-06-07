# DockerSwarm
Estos serian los host para Docker Swarm
Maestro 192.168.0.6
Nodo2 192.168.0.6
Nodo3 192.168.0.6
Nodo4 192.168.0.6
Una vez que haya iniciado sesión en su instancia de CentOS 7, ejecute el siguiente comando para actualizar el sistema base con los últimos paquetes disponibles.
yum update -y
Empezar
Antes de comenzar, deberá configurar el archivo /etc/hosts en cada nodo, para que cada nodo pueda comunicarse entre sí por nombre de host.
Puede actualizar el archivo /etc/hosts en cada nodo como se muestra a continuación:
nano /etc/hosts192.168.0.102  managernode
192.168.0.103  workernode1
192.168.0.104  workernode2
Guarde y cierre el archivo cuando haya terminado.
A continuación, deberá configurar el nombre de host en cada nodo según el archivo /etc/hosts.
Puede hacerlo ejecutando el siguiente comando en cada nodo uno por uno:
Nodo Maestro:
hostnamectl set-hostname maestro
Nodo de trabajo1:
hostnamectl set-hostname nodo1
Nodo de trabajo2
hostnamectl set-hostname nodo2

Nodo de trabajo3
hostnamectl set-hostname nodo3
Nodo de trabajo4
hostnamectl set-hostname nodo4

Instalar Docker Engine
A continuación, deberá instalar Docker Community Edition en todos los nodos. De forma predeterminada, la versión más reciente de Docker CE no está disponible en el repositorio de CentOS 7. Por lo tanto, deberá agregar el repositorio de Docker CE al sistema.
Puede hacerlo ejecutando el siguiente comando en todos los nodos:
wget https://download.docker.com/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker.repo
Una vez instalado el repositorio de Docker, ejecute el siguiente comando para instalar Docker CE:
yum install docker-ce –y
A continuación, inicie el servicio Docker y habilite que se inicie en el arranque mediante el siguiente comando:
systemctl start docker
systemctl enable docker
Configurar firewall
A continuación, deberá abrir los puertos 7946, 4789, 2376, 2377 y 80 en el firewall para que un clúster de enjambre funcione correctamente.
Ejecute el siguiente comando en todos los nodos:


firewall-cmd --permanent --add-port=2376/tcp
firewall-cmd --permanent --add-port=2377/tcp
firewall-cmd --permanent --add-port=7946/tcp
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=7946/udp
firewall-cmd --permanent --add-port=4789/udp



Por último, vuelva a cargar el firewall y el servicio Docker para aplicar todos los cambios:
firewall-cmd --reload
systemctl restart docker


Crear un enjambre




A continuación, deberá inicializar el enjambre en el nodo Manager. Puede hacerlo ejecutando el comando docker swarm init. Este comando hará que su nodo como nodo de administrador y la publicidad de su IP:
docker swarm init --advertise-addr 192.168.0.102
Debería ver la siguiente salida:


Swarm initialized: current node (viwovkb0bk0kxlk98r78apopo) is now a manager.To add a worker to this swarm, run the following command:    docker swarm join --token SWMTKN-1-3793hvb71g0a6ubkgq8zgk9w99hlusajtmj5aqr3n2wrhzzf8z-    1s38lymnir13hhso1qxt5pqru 192.168.0.102:2377To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
Nota: Recuerde el token de la salida antedicha. Esto se usará para unir nodos de trabajo al nodo de administrador más adelante.
Puede comprobar el estado del clúster de Swarm mediante el siguiente comando:
docker info






Unir los nodos de Worker al nodo Manager
El nodo Manager ya está listo. A continuación, deberá agregar el nodo Worker al nodo Manager.
Puede hacerlo ejecutando el comando docker swarm join en ambos nodos Worker de la siguiente manera:
docker swarm join --token SWMTKN-1-3793hvb71g0a6ubkgq8zgk9w99hlusajtmj5aqr3n2wrhzzf8z-1s38lymnir13hhso1qxt5pqru 192.168.0.6:2377






luego verificamos en nuestro maestro que se hallan agregado cada uno de los nodos
En el nodo manager, ejecute el siguiente comando para comprobar el estado del nodo, independientemente de si los nodos están activos o no: 
docker node ls



Sistemas 2
