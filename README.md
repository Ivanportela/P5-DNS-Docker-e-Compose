**Configuración da imaxe bind9 utilizando como medio Docker-Compose**

Primeiramente copiaremos copiaremos e configuraremos todo o que necesitaremos en local, unha vez feito isto seguiremos as instruccións proporcionadas nos enlaces da práctica.

**Docker-compose.yml, creación e configuración.**

Dentro do directorio da práctica utilizaremos o seguinte comando:
```
nano Docker-Compose.yml
```
Dentro deste introduciremos o còdigo de referencia que aparece no setup da imaxe:
```
BIND 9.18 (extended support version, aka 'old stable')

docker run \
        --name=bind9 \
        --restart=always \
        --publish 53:53/udp \
        --publish 53:53/tcp \
        --publish 127.0.0.1:953:953/tcp \
        internetsystemsconsortium/bind9:9.18
```

Afondando un pouco buscando información sobre isto aparecía que era moi común que o porto 53 que é o defecto neste caso o cambiemos o que podemos facer para solucionalo é o seguinte:

```
54:53/udp
54:53/tcp 
127.0.0.1:953:953/tcp 
```

**Unha vez añadidos os volumes anteriores ao yml quedaranos o seguinte código:**
```
services:
  #Servizos que se executarán no contenedor
  bind9: #Como queremos chamar nos o servizo
    image: internetsystemsconsortium/bind9:9.18 #Imaxe que utilizaremos no contenedor
    container_name: Practica5_bind9 #Nome específico do contenedor
    ports:
      #Mapeo dos portos host:contenedor
      - 54:53/udp
      - 54:53/tcp
      - 127.0.0.1:953:953/tcp #Neste só pertmite conexións desde localhost
    volumes:
      #Volumes onde se montará o contenedor
      - ./etc/bind:/etc/bind
      - ./var/cache/bind:/var/cache/bind
    restart: always #Esta opción indica que ó contenedor debe reiniciarse en ciertas condiciones
```
**named.conf**

Este é o arquivo principal do servidor polo que teremos que seguir os pasos asignado no, setup da imaxe, polo que teremos que escribir dentro o seguinte código.

```
options {
    directory "/var/cache/bind";
};
```
**Para comprobar que todo funcionou correctamente dentro do directorio de onde temos o arquivo yml lanzaremos o seguinte comando na terminal:

```
docker compose up -d #O -d serve para que se execute en segundo plano
```
E se todo funcionou correctamente debería aparecer o seguinte:

```
[+] Running 2/2
 ✔ Network <nome_da_network>  Created                                      0.1s 
 ✔ Container Practica5_bind9  Started                                      0.8s 
```


