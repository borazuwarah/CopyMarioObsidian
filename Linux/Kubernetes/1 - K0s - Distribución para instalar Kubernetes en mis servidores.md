## CONCEPTOS INTRODUCTORIOS

- CLUSTER -> Es un conjunto de máquina o de nodos.
- NODOS -> Cada una de las máquinas, por ejemplo un servidor Ubuntu.
- POD -> Dentro de cada nodo o máquina, puede haber pods que a su vez guarden varios contenedores a la vez.
-------
Lo primero será importar el certificado SSH en las máquinas donde queramos administrarlas con Kubernetes, por tanto lo haremos:
![[Pasted image 20230121032110.png]]
Ahora debemos activar la opción de poder conectarnos por ssh con el usuario root; y eso lo podemos configurar dentro del fichero /etc/ssh/sshd_config y activamos el PermitRootLogin:
![[Pasted image 20230121225400.png]]
EL siguiente paso será establecer una contraseña para el usuario root dentro de la máquina que vamos a administrar; y para ello utilizaremos el comando passwd root:
![[Pasted image 20230121231736.png]]
Y ahora copiamos este certificado a los servidores:
![[Pasted image 20230121225526.png]]
Y ya nos podremos conectar como root sin poner credenciales:
![[Pasted image 20230121225615.png]]
Ahora instalaremos k0s en la máquina anfitrión; y para ello ejecutaremos estos comandos:
```
wget https://github.com/k0sproject/k0sctl/releases/download/v0.15.0/k0sctl-linux-x64 && mv k0sctl-linux-x64 k0sctl&& chmod 777 k0sctl
```
Una vez ejecutado este comando, ya tendremois k0s:
![[Pasted image 20230121034137.png]]
Ahora debemos de crear un fichero de configuración en formato yaml para poder configurar el k0s dentro de los equipos que queramos, que en mi caso sólo administraré uno pero podría hacerlo con muchos más:
![[Pasted image 20230121232226.png]]
Y ahora debemos ejecutar este fichero yaml con el siguiente comando:
![[Pasted image 20230121232316.png]]
Por tanto ahora se nos lanzará el cluster con la máquina que hemos elegido administrar:
![[Pasted image 20230121232344.png]]
Ahora ya tenemos nuestro kluster desplegado, y si queremos ver los nodos primero tendremos que exportar un fichero .yaml y depsués visualizarlo, por tanto utilizaremos estos dos comandos:
![[Pasted image 20230121233159.png]]
## KUBECTL - Para administrar los clusters de Kubernetes
Ahora para poder administrar nuestros clusters, vamos a instalar kubectl, lo cual lo haremos con los siguientes comandos:
```
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl
```
En este punto también podemos utilizar el comando kubectl cluster-info --kubeconfig kubeconfig.yaml para conocer información del cluster al que me estoy conectando:
![[Pasted image 20230121233550.png]]
Y también dentro de los klusters que tenemos desplegados, podemos obtener más información sobre uno de ellos, yo por ejemplo en mi caso sólo tengo desplegado el cluster server, por lo que obtendré información de este mismo:
![[Pasted image 20230121233746.png]]
Ahora si queremos ver los recursos que tiene uno de nuestros nodos, utilizaremos este comando:
![[Pasted image 20230121234109.png]]
