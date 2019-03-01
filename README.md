# Fundamentos de Docker
Docker nos ayuda con varios problemas y uno de los más importantes es que nos evita pasar vergüenzas ya no volveremos a decir esta oración: "En mi máquina funciona bien".  
Lo que hace es que tal cual como nosotros tenemos funcionando una aplicación en nuestra máquina, así mismo va a funcionar en docker.  
Docker es un servicio que funciona con "contenedores" en los cuales se van a almacenar nuestras aplicaciones con todas las dependencias que necesitamos para que funcione correctamente.

## Instalación de docker

### Instalando en Windows y en Mac
Solo debes dirigirte a la [página de docker](https://www.docker.com/get-started) donde podrás encontrar las descargas para estos sistemas operativos y solo debes seguir las instrucciones

### Instalando en Ubuntu
Debes seguir los siguientes pasos para instalar docker en Ubuntu.

1. Debes actualizar tus repositorios.
~~~sh
$ sudo apt update
~~~
2. Intala los siguientes paquetes utilizando el siguiente comando.
~~~sh
$ sudo apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
~~~
3. Añade la llave GPG oficial de Docker.
~~~sh
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
~~~
4. Utiliza el siguiente comando para configurar el repositorio estable.  
Si quieres añadir las veriones de test o las nightly agrega la palabra test o nightly (o las dos) despues de la palabra "stable" en el siguiente comando.
~~~sh
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
~~~
5. Una vez que ya tenemos todo eso listo vamos a actualizar nuevamente nuestros repositorios.
~~~sh
$ sudo apt update
~~~
6. ahora instalamos los siguientes paquetes.
~~~sh
$ sudo apt install docker-ce docker-ce-cli containerd.io
~~~
7. Verificaremos que ya quedó instalado revisando la versión del Docker con el siguiente comando.
~~~sh
$ sudo docker --version
~~~
8. Siempre que querramos ejecutar docker debemos hacerlo como super usuario entonces para poder ejecutarlo normalmente debemos agregar un mod a nuestro usuario para que pueda utilizar Docker sin necesidad de usar sudo.
~~~sh
$ sudo usermod -aG docker <<tu_username>>
~~~
9. Por ultimo debes reiniciar la sesión y ya tendrás todo listo.

## Mi primer hola mundo con Docker
Una vez que ya todo está instalado y funcionando correctamente vamos a verificar que todo está bien haciendo nuestro primer "hola mundo" con docker, para ello vamos a ejecutar el siguiente comando
~~~sh
$ docker run hello-world
~~~ 
una vez hecho esto habremos ejecutado nuestro primer contenedor de docker y nos trae cierta información que indica que docker está funcionando correctamente en nuestro pc.

## Contenedores
Un contenedor es una agrupación de procesos, es una entidad lógica y a diferencia de las maquinas virtuales no es una simulación ni nada de eso, un contenedor ejecuta los procesos de forma nativa utilizando como base el sistema operativo y esto contenedores ven su universo como solo lo que hay en este contenedor y no tienen forma de consumir más recursos de los que el contenedor tiene disponibles.  
Un contenedor solo tiene acceso a una parte del disco delimitada y no tiene acceso a nada más, no va a saber que existe más hardware del asignado.  
[Aquí](https://itnext.io/chroot-cgroups-and-namespaces-an-overview-37124d995e3d) podremos encontrar mucha documentación donde se habla a cerca de que componentes del kernel de linux utiliza docker.
## Comandos
Vamos a ver una serie de comandos que nos ayudarán en el camino de entender como funciona docker.

| Comando | Descripción |
|---------|-------------|
|docker ps|Lista los contenedores en ejecución|
|docker ps -a|Lista los contenedores a detalles|
|docker ps -qa|Lista solo los Id de los contenedores (q: quiet, tranquilo o silencioso)|
|docker inspect [id_contenedor]|Muestra los detalles de un contenedor en específico|
|docker inspect [nombre_del_contenedor]|Muestra los detalles de un contenedor con el nombre ingresado|
|docker inspect -f [json template para el filtrado] [id_contenedor]|Muestra el detalle de un atributo del contenedor seleccionado|
|docker rename [nombre del contenedor] [nuevo nombre]|Renombra el contenedor seleccionado|
|docker rm [nombre del contenedor]|Elimina el contenedor con el nombre|
|docker rm $(docker ps -aq)|Elimina todos los contenedores que no esten corriendo|
|docker run ubuntu tail -f /dev/null|Ejecutamos un contenedor con Ubuntu y lo dejamos prendido y a la espera de que querramos ingresar|
|docker exec -it [nombre de contenedor con ubuntu] bash|Permite ingresar a ese contenedor para seguir trabajando allí|
|docker kill [nombre del contenedor]|mata a ese contenedor si se está ejecutando|
|docker rm -f|Forzar la eliminación de un contenedor|
|docker run -d --name server nginx|ejecuta un servicio web a traves de nginx|
|docker run -d --name server -p 8080 nginx|ejecuta un servicio web a traves de nginx y se le dice explicitamente que el puerto 8080 de mi máquina apunte al servicio de nginx|
|docker run -d --name db mongo|ejecuta un servicio de base de datos con mongo|

## Corriendo un ubuntu en docker
Dentro de docker podemos hacer muchas cosas y una de las mas locas es que podemos correr un sistema operativo como ubuntu dentro de docker y solo debemos escribir el siguiente comando.  
~~~sh
$ docker run -ti ubuntu
~~~
Nos aparecerá la shell de ubuntu y ahí podremos ejecutar todos los comando de ubuntu sin ningun problema.

## Ciclo de vida de un contenedor
Un contenedor siempre estará vivo mientras se esté ejecuntando un proceso dentro de él, si nos salimos y no podemos entrar nuevamente lo unico que debemos hacer para cerrar ese proceso es decirle a docker que mate los procesos del contenedor.  

## Cómo exponer un contenedor al mundo
Lo vamos a hacer utilizando [nginx](https://www.nginx.com/) y con los comandos que se muestran en la tabala para crear un nuevo servicio.

## Datos en docker
Para este caso vamos a utilizar una base de datos de mongo y para ello utilizamos el siguiente comando
~~~sh
$ docker run -d --name db mongo
~~~
Lo que hicimo fu crear un nuevo contenedor con una base de datos mongo que se va a llamar db y que va a funcionar aunque no la veamos gracias al flag -d.  
Uno de los problemas al hacer esto es que si eliminamos el contenedor, toda la data almacenada en él va a ser eliminada tambien. Para que eso no suceda debemos crear un direcctorio en nuestro sistema operativo que se va a conectar al contenedor como un volumen externo y ahí se almacenarán todos los datos.  

1. Crearemos un directorio para los datos de nuestro contenedor
~~~sh
$ mkdir mongodata
~~~
2. vamos a iniciar nuestro contenedor especificandole cual es el directorio que va a utilizar y lo hacemos de la siguiente manera
~~~sh
$ docker run --name db -d -v [el path de nuestra carpeta mongodata]:[destino dentro del directorio] mongo
// En mi caso yo lo hice así:
$ docker run -d --name db -v /home/neox/Projects/fundamentos_docker/mongodata:/data/db mongo
~~~
~~~
