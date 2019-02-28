# Fundamentos de Docker
Docker nos ayuda con varios problemas y uno de los más importantes es que nos evita pasar vergüenzas ya no volveremos a decir esta oración: "En mi máquina funciona bien"
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
4. Utiliza el siguiente comando para configurar el repositorio estable. Si quieres añadir las veriones de test o las nightly agrega la palabra test o nightly (o las dos) despues de la palabra "stable" en el siguiente comando.
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
