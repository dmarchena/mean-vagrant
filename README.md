mean-vagrant
============

Configuración sobre [Puppet](https://puppetlabs.com/) de un entorno de desarrollo virtualizado para aplicaciones web en stack MEAN (MongoDB, Express, AngularJS y Node.js).


### Requisitos
Para utilizar el proyecto necesitas tener instalado:
* Un entorno de virtualización, como VirtualBox 
* [Vagrant](http://www.vagrantup.com/) (v1.1+)
* Con VirtualBox, es recomendable instalar el plugin [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest/) para Vagrant  
`$ vagrant plugin install vagrant-vbguest`


#### ¿Qué es Vagrant?
Es una herramienta de código abierto (disponible para OS X, Windows y Linux) que te permite crear entornos de desarrollo sobre los que simular una máquina similar al servidor en que se alojará una aplicación web. También resulta una herramienta muy util para compartir un entorno ya configurado entre diferentes equipos, con la ventaja de no ensuciar el sistema operativo de ninguno de ellos.


### El entorno
Esta configuración levanta dos [boxes](http://docs.vagrantup.com/v2/boxes.html) Ubuntu 12.04 (Precise Pangolin) 64-bit. El primero, llamado `mongodb`, como servidor de BBDD; y el otro (`appserver`), como servidor de aplicaciones.

Las direcciones IP de dichas máquinas serán:

* Servidor de aplicaciones: `192.168.56.12`
* Servidor de MongoDB: `192.168.56.11`

A continuación se muestran los paquetes que se instalan en ambas:
* Box mongodb
    * [MongoDB](https://www.mongodb.org/)
* Box appserver
    * [Node.js](https://nodejs.org/)
    * Módulos globales de NPM  
    _Nota: si deseas saber las versiones instaladas podrás ejecutar `npm list -g --depth=0` conectandote a appserver vía SSH (`vagrant ssh appserver`) despues de lanzar las máguinas_
	    * [Bower](http://bower.io/)
	    * [Express](http://expressjs.com/)
	    * [Jade](http://jade-lang.com/)
	    * [Jasmine-node](https://github.com/mhevery/jasmine-node)
	    * [Mongoose](http://mongoosejs.com/)
	    * [Nodeunit](https://github.com/caolan/nodeunit)


### El workspace

Con Vagrant, mediante [Synced Folders](http://docs.vagrantup.com/v2/synced-folders/) también tendremos la posibilidad de compartir una carpeta entre la máquina Host (nuestro equipo) y la máquina Guest (en este caso, el box `appserver`). Eso nos permitirá trabajar sobre los archivos de las aplicaciones desplegadas sin tener que conectarnos a la máquina virtual por SSH.

En esta configuración, la carpeta compartida es un directorio llamado `workspace` que será creado como hermano del directorio de este proyecto.

```
parent/
    mean-vagrant/
    workspace/
```

Si se desea cambiar el nombre de dicha carpeta compartida o su localización habrá que cambiar la siguiente instrucción dentro del archivo `Vagrantfile` que se encuentra en el directorio raiz de este proyecto:

```
appserver.vm.synced_folder "../workspace", "/usr/local/src/mean", create: true
```

### Lanzar las máquinas

1. Haz un clone del repositorio: `git clone git://github.com/dmarchena/mean-vagrant.git`
2. Ejecuta el comando `vagrant up` desde el directorio que se acaba de crear. Éste será el comando que deberás ejecutar cada vez que quieras levantar los servidores.  
La primera ejecución podrá tardar bastante mientras Vagrant descarga todos los recursos necesarios y realiza la configuración inicial de los servidores. Sé paciente, las siguientes veces apenas tardará. ;) 
Sabrás que el proceso ha llegado a su fin correctamente tras unas líneas similares a las siguientes:  
    ``` 
    ==> mongodb: Machine 'mongodb' has a post `vagrant up` message. This is a message
    ==> mongodb: from the creator of the Vagrantfile, and not from Vagrant itself:
    ==> mongodb: 
    ==> mongodb: mongodb listening on 192.168.56.11
    
    ==> appserver: Machine 'appserver' has a post `vagrant up` message. This is a message
    ==> appserver: from the creator of the Vagrantfile, and not from Vagrant itself:
    ==> appserver: 
    ==> appserver: appserver listening on 192.168.56.12
    ```
3. Copia tu aplicación dentro del directorio de trabajo
4. Conectate a la terminal del servidor de aplicaciones ejecutando el comando `vagrant ssh appserver` desde el mismo directorio del paso 1.
5. En la nueva terminal, ejecuta los comandos que necesites para desplegar (bower, npm...) y lanzar tu app con Node.js
6. La aplicación estará a la escucha en: http://192.168.56.12:3000/  _(suponiendo que tu aplicación utilice el puerto 3000)_


### Algunos comandos útiles de Vagrant

* `vagrant suspend`: Pausar las máquinas
* `vagrant resume`: Reanudarlas
* `vagrant halt`: Apagar totalmente los dos servidores
* `vagrant destroy`: Apagar las máquinas y libera espacio eliminando todos los recursos que se descargaron en el primer arranque.  
_Nota: Esto no borra la configuración de este proyecto ni tu workspace, por lo que con un nuevo `vagrant up` podrás instanciar ambas máquinas y volverlas a arrancar._


### Licencia

Este código está disponible con licencia GNU General Public License v3.0 Échale un vistazo al archivo [LICENSE.md](https://github.com/dmarchena/mean-vagrant/blob/master/LICENSE.md) para más información.
