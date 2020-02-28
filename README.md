# HAProxy

<div class=text-justify>


En [Vagrantfile](Vagrantfile) se encuentra el archivo que describe la definición de las tres máquinas virtuales que se requieren para hacer el despliegue del cluster de máquinas indicados en este [enlace](https://www.howtoforge.com/tutorial/ubuntu-load-balancer-haproxy/).

Modifique el `Vagrantfile` de modo que las máquinas virtuales se creen con el sistema operativo que indica el [enlace](https://www.howtoforge.com/tutorial/ubuntu-load-balancer-haproxy/).

---

HAProxy es un software de código libre desarrollado en C, que provee un balanceador de carga (Load Balancer) y servidor proxy para aplicaciones basadas en TCP y HTTP. El propósito de este software principalmente es distribuir el tráfico y generar un proxy inverso para proteger la base de datos de ataques externos. Está orientado para sitios web con alto tráfico y es usado en los sitios web más visitadas del mundo.

Para el despliegue del cluster, contamos con 3 máquinas virtuales de ubuntu/xenial64 cada una con 386 kb de memoria y un núcleo de procesamiento. A continuación se explicará el Vagrantfile.

![Texto alternativo](/screenshots/1.png)

El anterior Vagrantfile configura las 2 máquinas virtuales que harán de servidores web, web.vm.network define una red privada a nivel local y se asigna una dirección ip, web.vm.provision ejecuta por shell dentro de la máquina virtual el script web.sh para aprovisionar de las herramientas necesarias para montar el servidor web, web.vm.synced_folder define un directorio sincronizado en la máquina virtual con la computadora local, este lleva todos los datos de la página web.

![Texto alternativo](/screenshots/2.png)

En esta parte del Vagrantfile se configura el servidor de HAProxy, la diferencia con las máquinas anteriores, es que al levantar la máquina, la configuración web.vm.network hace redireccionamiento para enviar el tráfico por un puerto definido en el host (8080), a otro puerto definido en la máquina destinataria (80), también se ejecutará por shell el script haproxy.sh.

![Texto alternativo](/screenshots/3.png)

web.sh ejecuta por bash la instalación del servidor web apache.

![Texto alternativo](/screenshots/4.png)

Ejecuta por bash la instalación de HAProxy, en el script, la línea con ENABLED=1 habilita que haproxy sea levantado por el script init.

![Texto alternativo](/screenshots/5.png)

Este bloque configura donde deben hacerse los mensajes de los registros, la cantidad de concurrencias permitidas en el front end y el usuario y el grupo que no deben ser cambiados.

![Texto alternativo](/screenshots/6.png)

En este bloque se define el modo en que se va usar haproxy (http) y los tiempos máximos de espera para una conexión del cliente o el servidor.

![Texto alternativo](/screenshots/7.png)

Este bloque contiene la configuración entre el backend y el frontend, haproxy está configurado para utilizar el puerto 80, webfarm es el nombre que le damos a la aplicación (la granja de servidores) y las configuraciones necesarias para el balance y las direcciones del back end.

Para llevar a cabo esta **configuración** deberá seguir los pasos que están en la [guía de instalación sugerida](https://www.howtoforge.com/tutorial/ubuntu-load-balancer-haproxy/).

---

**Nota** Hay un error en el archivo de configuración sugerido por la página web. 
Existe una línea que dice:

```
listen webfarm 0.0.0.0:80
```

Esta se debe cambiar por estas dos:

```
listen webfarm 
bind 0.0.0.0:80
```

Una vez la configuración este lista, asegurarse que se puede hacer el acceso al servidor de HAProxy desde el número IP del *host*.

</div>
