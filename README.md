# Lame-HTB


Esta es una maquina facil lo que quiero aqui es crear una especie de algoritmo para la resolucion de todas las maquinas en el futuro. ¡PACIENCIA es lo que falta!

## Paso 1 NMAP

Ver puertos abiertos se hace asi porque es mas rapido.

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/0d46bef3-60e4-46bb-bf7e-730455ecbd60)


```
nmap -sS --open -Pn --min-rate 5000 -p- -vvv  10.129.171.99 -oG allports 
```

Sobre esos puertos ahora si se le avienta los scripts basicos de enumeracion.

```
sudo nmap -sSCV -p 21,22,139,445 10.129.171.99 -oN Scan

```

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/cb38b7cb-cb55-482d-b1da-f13e242f5e36)


PENSAMIENTO: No existe ningun puerto para algun servidor web entonces tenemos que ver si algun servicio es vulnerable.... Vamos a empezar con el ftp.

## Paso 2 si no hay algo web REVISAR SERVICIOS VULENRABLES

### Searchsploit

Para actualizar la base de datos ( hacerlo regularmente)

```
searchsploit -u
```

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/0bc9f91a-bdca-4657-976c-38b544afdf0a)

```
searchsploit vsftpd 2.3.4
```

Copiar el exploit a la carpeta de trabajo.

```
searchsploit unix/remote/49757.py
```

Para ver el contenido del exploit

```
searchsploit -x unix/remote/49757.py
```

Pero no te limites a searchsploit busca en internet

### vsftpd-2.3.4 server

En 2011 se encontro que alguien backdoorizo el parquete de este servidor. La vulnerabilidad consistia en que un usuario metiera una :) (si una cara feliz y cualquier password y se regresaba una shell en el puerto 6200 ( el cual en esta maquina esta cerrado de todas maneras intentalo.)

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/7be9e847-b6f5-4ed1-aa73-64c853709140)

Ojo te dice una shell no una reverse shell y ahi tienes lo que hace el codigo de bajo pero por ejemplo si lo hicieras a mano...

## BATCAT

Usa esta herramienta para ver los codigos con colores e incluso puedes ponerle que lenguaje es y pues te lo colorea.

```
batcat 49757.py -l python
```

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/b1a9541f-b50a-4ad6-a139-c20962c2d902)

## SAMBA 3.20 

Esta version de SAMBA es vulnerable entonces revisando el exploit de ruby(msfconsole) vemos que intenta inyectar comandos. Hay que revisar los shares

## SMBCLIENT

Para revisar sesiones nulas se hace asi:

```
smbclient -L 10.129.171.99 -N --option  'client min protocol = NT1'
```

Se busco en internet el error que arrojo y por eso asi se puso. Para conectarse

```
smbclient  //10.129.171.99/tmp -N --option  'client min protocol = NT1'
```

## RCE

La version de SAMBA es vulnerable (si miramos el exploit) 

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/bb194c3a-ca18-427f-b707-f85fd05a729c)

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/9cac2815-8659-4bd3-873e-37b7f729617d)


Al momento de ejecutarse pues inyecta un comando el cual si ponemos a la escucha un tcpdump.

### TCPDUMP

```
sudo tcpdump -i tun1 icmp -n

```

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/4bdcbdd0-50d4-4049-8815-85ef93a1d903)

Vamos a probar con SMBCLIENT.

Conectandote ya es pan comido

![image](https://github.com/gecr07/Lame-HTB/assets/63270579/b1877f29-e05e-450c-9b18-b4bbdd0bef78)




