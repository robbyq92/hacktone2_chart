# hacktone2_chart
Crear un chart de Helm

# 002 Helm Chart Web
 
Vas a crear un Helm Chart para desplegar una aplicación web llamada "WebAppX" (donde X será un número a elegir por vosotros). La aplicación es simple y consta de un servidor web que sirve una página HTML básica que podéis encontrar en esta imagen (usadla!): https://github.com/cloudogu/hello-k8s
 
Requisitos de la Aplicación:
Servidor Web:
La aplicación utiliza un servidor web https://github.com/cloudogu/hello-k8s.
Haced por forwading para ver la ver la web.
Página HTML:
La imagen ya viene con un servidor web y un contenido customizable.
 
Requisitos de Chart de Helm:

Valores Configurables:

```
Nombre de la aplicación.
Puerto donde escucha la aplicación.
Mensaje (mirar documentación de la imagen hello-k8s).
Posibilidad de cambiar el número de replicas.
```
Una vez desplegada la primera aplicación, desplegar 2 más con el mismo Chart solo cambiando el nombre, el mensaje y el puerto de la aplicación.
 
TIPS:
helm create
-f

## PASOS:
Primero se crea el chart de helm con helm create WebApp1, donde crea por defecto unos ficheros de configuración que nos ayuda a configurar cada cosa (deployment, servicio, service account, etc). Los ficheros los podemos configurar a nuestro gusto, ya que los primeros, son por defecto para que nos hagamos una idea de como están las variables configuradas.

```
helm create Webapp1
```
Nos crea la siguiente carpeta, donde luego borramos lo que no nos hace falta para configurar el chart creado y customizarlo.

Vemos las carpetas:

![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/facdb952-f8dd-4e81-b983-1a506e0396d8)

Dentro tenemos varios archivos:

![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/35981132-d8d5-4bcb-b204-b0b6286aad48)

Las carpetas y ficheros:
```
- Chart.yml -> Es el fichero de configuración donde podemos escribir las versiones del chart y la descripción del chart.
- Values.yml -> Es el fichero de configuración donde pondremos nuestros variables por defecto, para luego inyectarlos en los distintos servicios (deployment, services, etc) que se encuentra en Templates.
- Templates -> Carpeta con los ficheros .ymls de configuración, según si es un deployment, servicio, ingress, etc.
NOTA: Hay un fichero de ignore, en esto se deja todo lo que se quiere ignorar, tipo cosas de ficheros de backup, git, etc.
```

Dentro de la carpeta Templates, se ha dejado los ficheros ymls y un .tlp que es un fichero de templates (plantilla).

![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/36d7540e-f530-4b29-bf4b-5758956e636d)


## Despliegue
Una vez dentro de la carpeta WebApp1 en este caso, podemos ejecutar el siguiente comando:
```
helm install webapp1 . --dry-run  -n hacktone2-helm
```
El comando en si, instala el chart, pero en modo --dry-run para comprobar primero que funciona todo y no encuentra errores, en el Namespace "hacktone2-helm" dandole un nombre al chart "webapp1".

Las flechas y los bloques, se puede observar que al desplegarlo saldría con el nombre de la configuración por defecto.

![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/cdfc418f-e6e2-42af-babf-4a85de5ff93f)

NOTA: Para los requerimientos se pedía que se pudiese cambiar el nombre, el puerto, replicas, por lo tanto vamos a modificarlo con los parametros de --set y ver si cambia
los valores requeridos:

```
 helm install webapp2 . --dry-run --set PORT=8082 --set MESSAGE=WebApp2 --set REPLICAS=2 --set NOMBRE_APP=WebApp2 -n hacktone2-helm
```
Ejecutando el comando anterior, alteramos las Replicas, Mensage, Puerto y Nombre de la App.


![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/0b40c91a-cd09-4c26-a490-28127bd51c74)

Lo mismo para el deployment 3, cambiando los tipos de Replicas, Nombre de la App y su puerto.

```
helm install webapp2 . --dry-run --set PORT=8083 --set MESSAGE="Deploy WebApp3" --set REPLICAS=5 --set NOMBRE_APP=WebApp3 -n hacktone2-helm
```

![image](https://github.com/robbyq92/hacktone2_chart/assets/49034238/acd76123-0588-40c0-9c7e-31bc7501a45b)

## Comprobaciones
Una vez que tenemos el dry run, hay que hacer los despliegues para ver que conviven todos en el mismo Namespace y que funcionan todos según el puerto y el servicio. Para ello los desplegamos con los comandos anteriores pero sin el dry-run.




