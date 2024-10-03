# Política de contraseñas

## Contexto
Por defecto, la mayoría de sistemas operativos, vienene con una política de contraseña muy laxa, una contraseña segura presenta una barrera más al atacante, que en su intento de acceder al sistema, indudablemete dejará más rastro y en algunos casos a provocar el abandondo de ataque.

## Objetivos
* Entender el sistema de contraseñas del sistema operativo linux
* Entender el PAM (Plugable authentication Module)
* Instalar y configurar la libreria _pwquality_
* Comprobar y verificar que se ha llevado la práctica de forma correcta
* Documentar la práctica.

## Entendiendo sistemas de contraseñas Linux

Linux guarda las contraseñas en el archvo `/etc/shadow`, las contraseñas almacenadas en este archivo pasan por un proceso de codificación, la codificación cumple los siguientes requisitos:

1. La contraseña es cifrada mediante un hash, este es una funcion unidreiccional.
2. De forma usual se emplea SHA-512 como algoritmo de hashing, tambien se pueden usar otros como el SHA-256 o el MD5.
3. También se usa la tecnica de salting, esta consiste en añadir un valor aleatorio antes de el hashing este valor se almacena junto a la contraseña y nos porteje de ataques de diccionario y de rainbow table.

Por otro lado `/etc/passwd` contiene los usuarios y cierta informacion util como por ejemplo los grupos a los que pertenecen.

## Entender el PAM

PAM (Pluggable Authentication Modules) es la infraestructura que se encarga de administrar la autenticación entre el sistema y los usuarios.


## Instalar y configurar el pwqality
Instalamos el pwquality mediante el comando `sudo apt install libpam-pwquality`

Estableceremos los diferentes tipos de politica de contraseñas:
+ Debil: 
    + Longitud minima: 6
+ Media: 
    + Longitud minima: 8
    + Contiene al menos un número: Si
+ Fuerte:
    + Longitud minima: 12
    + Contiene al menos un número: Si
    + Contiene al menos una mayuscula: Si
    + Contien un caracter especial: Si

#### Configuración del archvio `/etc/pwquality.conf`

Debil:
```bash
 minlen = 6
```

Media:
```bash
 minlen = 8
 minclass = 1
```

Fuerte:
```bash
 minlen = 12
 dcredit = -1
 ucredit = -1
 lcredit = -1
 ocredit = -1
```

|   | Score | Longitud minima | Requisitos | Ejemplos |
|:-:|:-----:|:---------------:|:----------:|:--------:|
| no valida| <40| -| Ninguno| jsyw27 sda2na carthe|
| débil| 40-50| 6| Minimo 6 caracteres| Hol@14 vin;c0 oP+2as|
| media| 50-80| 8| Minimo 8 caracteres y un número| T0uriÑlo Güi11em3 Marga1x|
| fuerte| >80| 12| Minimo 12 caracteres, un número, una mayúscula y un caracter especial| Hksuys864^a. jT5798hdA._1 7Qj75ga+Çofg|


## Comprobar que el pwqueality esta activo

Para comprobar que el pwquality esta activo debemos de revisar el archvio `/etc/pam.d/common-password`, y buscaremos las siguientes lineas, como podemos ver toma como requisito la libreria pam_pwquality.

```bash
password        requisite                       pam_pwquality.so retry=3
password        [success=1 default=ignore]      pam_unix.so obscure use_authtok try_first_pass yescrypt
```




