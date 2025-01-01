# Cómo hacer `push` a gitlab

Primero creamos nuestro proyecto
```sh
mkdir project
cd project/
touch file_one file_two
```

Ahora vamos nuestra carpeta `~/.ssh` y generamos nuestro archivo de acceso, que por lo regular está en nuestro directorio `HOME`:
```sh
ssh-keygen -t ed25519 -C "<comment>"
```
Nos pedirá un `passphrase` lo puedes dejar en blanco. pero por seguridad, inserta una contraseña.

Ahora bien, crearemos el archivo `config` en nuestra carpeta `~/.ssh` e insertaremos lo siguiente:
```js
Host gitlab.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_com_rsa
```

Guardamos y salimos, daremos los siguientes permisos:
```sh
chmod 644 config
```

Ahora en la página de `gitlab`, nos iremos a las preferencias de nuestro perfil, y ahí estará un apartado llamado `SSH Keys`, dentro de este daremos en el botón que dice `add new key` en la caja de texto `key` pegaremos lo que viene dentro de nuestro archivo `id_ed25519.pub` que está dentro de `~/.ssh`. Llenaremos los datos que se nos piden ahí como el uso y la fecha de caducidad.

En nuestra terminal escribimos lo siguiente:
```sh
ssh -T git@gitlab.com
```
Nos sacara un mensaje de aviso para aceptar la conexión remota y escribiremos `yes` y daremos `ENTER`. Si todo sale bien, nos mandará un mensaje de bienvenida.

Creamos nuestro repositorio en la web de `gitlab`. En la página de nuestro repositorio, iremos al apartado de `settings`, en el menú que se desplega daremos en `repository`, nos abrirá una página en la cual vendrá una lista de opciones, iremos a donde dice `protected branches` y habilitaremos `Allowed to force push`. En la página principal de nuestro repositorio, vamos al botón azul de la parte superior derecho que dice `code` y copiamos la `url` que dice `Clone with SSH`

Configura los siguiente parámetros antes de continuar si es que aún no lo haz hecho:
```sh
git config --global user.name "<username>"
git config --global user.email "<email>"
```

Ahora en la carpeta de nuestro proyecto daremos lo siguiente:
```sh
git init
git add .
git commit -m "first commit"
git branch -M main
```

Pegaremos la `url` que copiamos en `Clone with SSH` por delante del comando para agregar el repositorio remoto:
```sh
git remote add gitlab git@gitlab.com:<group>/<repo_name>.git
```

Y haremos un `push` de manera forzada poniendo el flag `-uf`:
```
git push -uf gitlab main
```
Esto debería subir en automático sin pedirnos usuario y contraseña.




