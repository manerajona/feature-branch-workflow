# The feature-branch Workflow

El flujo de trabajo asume una rama central "main" que representa la historia oficial del proyecto. 

En lugar de trabajar directamente en la rama main, los desarrolladores crean una nueva rama (a partir del último commit en main) cada vez que comienzan a trabajar en una nueva funcionalidad (feature). 

Las ramas de feature deben tener nombres descriptivos, como **feature/OT149-X**. La idea es dar un propósito claro y muy enfocado a cada rama. Además, las ramas de feature pueden (y deben) ser integradas a main mediante "pull-requests".

&nbsp;

## Branch de larga duración (main)

Este flujo de trabajo, como ya mencionamos, consiste en una única rama de larga duración conocida como rama "main". Los desarrolladores crean nuevas ramas de corta duración cada vez que empiezan a trabajar en una nueva *feature/bug/refactor/test*.

Este tipo de flujo de trabajo es conocido como "trunk-based" y es una práctica común entre los equipos de DevOps, que requiere que los desarrolladores integren ramas de corta duración en un "tronco" central o rama principal.

### Main (sin tags)
Esta rama se utiliza para el desarrollo. Cada desarrollador crea un branch desde lo último en main para trabajar en un *feature/bug/refactor/test*, luego va a subir un Pull Request para hacer merge de sus cambios en main, siempre **como un único commit (squash and merge)**.

### Main con tag Release Candidate
Esta rama se utiliza para las pruebas de integración y demos. Cuando se quieren pasar suficientes cambios a release candidate se hace un "tag-cut" como se muestra a continuación: 

> ***v1.0-rc1 -> v1.0-rc2 -> v1.0-rc3***. 

Una vez que se produce el "tag-cut" los cambios son candidatos para la próxima release.

### Main con tag Release
Esta es la rama que tiene el último código listo para producción. Una vez que todas las pruebas se han realizado y sabemos que estamos ante una versión estable, se hace un "tag-cut" de release como se muestra a continuación: 

> ***V1.0-rc3 -> v1.0***.

&nbsp;

## Branches de corta duración

### Features

Este tipo de branch lo utilizamos cuando estemos trabajando en una funcionalidad particular que se agrega a la aplicación. 
> Ejemplo: **feature/OT149-123**

### Refactors 

Este tipo de branch lo utilizamos cuando no estemos agregando ningún tipo de funcionalidad, sino que modificamos nuestro código para que se más mantenible, escalable, perfomante o para mejorar la seguridad. 
> Ejemplo: **refactor/OT149-124**

### Bug Fixes

Este tipo de branch se utiliza para solucionar algún bug encontrado que necesita ser corregido en la rama principal. 
> Ejemplo: **bugfix/OT149-125**

### Hot Fixes

Este tipo de branch raramente se utiliza, significa que hay que solucionar un bug en producción que necesita ser integrado cuanto antes. 
> Ejemplo: **hotfix/OT149-126**

### Tests

Este tipo de branch se utiliza cuando queremos agregar tests automatizados a la aplicación. 
> Ejemplo: **tests/OT149-127**

&nbsp;

## Pull-requests

Para crear un PR, abrir en el navegador el repositorio en github.com e ir a la solapa que dice *Pull request* (a la derecha de *Issues*). Luego seleccionar "New pull request", dejar el "base" en main y compararlo con el branch que queremos integrar.

Como título de PR usar la siguiente convención de nombre:

> **[#TICKET] Descripción breve**

Ejemplo: **[OT149-123] Added Users documentation**

&nbsp;

## Flujo de trabajo

Es importante que todos los cambios que se pasen a main sean integrados mediante pull-request y que **solo sean agregadas las funcionalidades que están pleaneandose para la próxima release**, con el fin de no cortar con el flujo de trabajo. Seguir la siguiente receta:

1. Moverse al branch de main y bajarse lo último en la rama.

```sh
$ git checkout main
$ git fetch origin
$ git pull
```

2. Crear un nuevo branch y publicarlo.

```sh
$ git checkout -b feature/OT149-X
$ git push -u origin feature/OT149-X
```

3. Aplicar los cambios sobre ```feature/OT149-X``` y hacer ```git commit -m``` con un comentario significativo.

4. Subir los cambios en el branch haciendo ```git push``` y crear un *pull-request* a *main*.

5. Integrar a *main* con *Squash and Merge* (como un único commit).

&nbsp;

## Troubleshooting

### 1. Configurar SSH

- Generar llaves rsa.

```sh
$ ssh-keygen -t rsa
Enter file name to save the key: id_rsa
(Optional) Enter passphrase
```

- Copiar la llave generada **id_rsa.pub**.

- En GitHub ir a settings > SSH and GPG keys > add new > (paste public key)

- Agregar llaves **id_rsa** y **id_rsa.pub** a **~/.ssh** (en caso de haberlas generado en otro directorio)

```sh
$ mv id_rsa* ~/.ssh
```

- Test ssh.

```sh
$ ssh -T git@github.com       
(Add to trusted Host)
(Enter passphrase if needed)
Hi manerajona! You have successfully authenticated, but GitHub does not provide shell access.
```
&nbsp;

### 2. Actualizar la historia de mi branch

```sh
$ git reset --soft [commitID before your commits start]

$ git stash save

$ git fetch origin

$ git pull origin main

$ git stash pop

$ git commit -m "Squashed"

$ git push --force
```
