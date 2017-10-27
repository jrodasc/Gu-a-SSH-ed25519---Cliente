# Gu-a-SSH-ed25519 - Cliente

Guía para acceder por SSH configurando desde el Cliente.

#### 1. Generar llave SSH manualmente

Las siguientes instrucciones para generar y cargar manualmente una llave SSH en el equipo local y poder acceder a los Servidores.

Una clave SSH consite en un par de archivos.Una es la clave privada, que nunca se debe compartir con nadie. El otro es la clave pública. El otro archivo es una clave pública que le permite iniciar sesión en el servidor. Cuando genere las claves, usará ssh-keygen para almacenar las claves en un lugar seguro, y evitar el inicio de sesión cuando se conecte a las instancias correspondientes.

Para generar llaves SSH en Linux, siga los siguientes pasos:En el equipo local ejecutar el siguiente código
        ssh-keygen -A
  ssh-keygen -t ed25519
  eval `ssh-agent -s`
  ssh-add
  ssh-add ~/.ssh/id_ed25519

La llave SSH se ha creado, ahora es necesario visualizar dicha llave y enviarlo al
administrador para que se guarde las credenciales en el servidor y permita el acceso al
servidor, para dicha acción agregamos la siguiente linea de código en la consola:

  cat ~/.ssh/id_ed25519.pub
  
Y mostrará en pantalla una lllave SSH:
  
  ssh-ed25519 AAAAC3Nzavrg545t64t34rferodwEv1MMI2+Nh9QxpgrNxb2Is1Cc6 test1

Esa llave debe ser enviado al administrador para que sea ingresado al servidor.

#### 2. Ingresar por SSH al servidor

Para ingresar por SSH al servidor, es importante mencionar que se va ingresar por el
puerto 4449, y el comando a utilizar es el siguiente:

  ssh –p 4449 administrador@192.168.10.1

Con eso ya ingresamos al servidor.
