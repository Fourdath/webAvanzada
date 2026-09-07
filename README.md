# webAvanzada



## Laboratorio Git, Angular, CI/CD y Terraform



### Pregunta 1

No se recomienda desarrollar directamente sobre `main` porque esta rama debe mantenerse estable. Trabajar en una rama separada permite realizar cambios, probarlos y validarlos mediante un Pull Request antes de incorporarlos a la rama principal.



### Pregunta 2

El parámetro `--skip-git` evita que Angular cree un segundo repositorio Git dentro de la carpeta `frontend`. El repositorio Git ya existe en la raíz de `webAvanzada`, por lo que crear otro `.git` produciría un repositorio anidado.



### Pregunta 3

`npm run build` verifica que la aplicación Angular pueda compilarse correctamente y generar los archivos necesarios para su distribución. También permite detectar errores de compilación antes de automatizar el proceso mediante CI.



### Pregunta 4

Revisar `git status` y `git diff --cached` permite comprobar qué archivos y cambios serán incluidos en el commit, evitando versionar archivos innecesarios o información que no debería incorporarse al repositorio.



### Pregunta 5

El workflow `ci.yml` se activa mediante el evento `pull_request` cuando se crea o actualiza un Pull Request cuyo destino es la rama `main`.



### Pregunta 6

`ubuntu-latest` representa la imagen de un runner de GitHub Actions basado en Ubuntu sobre el cual se ejecuta el job `frontend`.



### Pregunta 7

Las etapas de validación del job `frontend` se ejecutan en el siguiente orden:



1. Obtener el código.

2. Configurar Node.js.

3. Instalar las dependencias con `npm ci`.

4. Ejecutar las pruebas.

5. Construir la aplicación Angular.



`npm ci` debe ejecutarse antes de las pruebas porque instala de manera reproducible las dependencias definidas en `package-lock.json`, las cuales son necesarias para ejecutar correctamente las pruebas y el build.



### Pregunta 8

La etapa que falla es `Ejecutar pruebas`, debido a que se modificó temporalmente la prueba para esperar el texto `Título incorrecto`, mientras que la aplicación realmente muestra `Catálogo de Recursos`.



Al fallar esta etapa, el job se detiene y la etapa posterior `Construir Angular` queda omitida.



### Pregunta 9

No se debe integrar el Pull Request a `main` mientras el pipeline esté fallando, porque los cambios no han superado las validaciones automáticas. Integrarlo en ese estado podría incorporar código defectuoso o no validado a la rama principal.



### Pregunta 10

Clasificación:



- `package.json`: versionable.

- `API_URL` pública: variable/configuración.

- `AWS_REGION`: variable/configuración.

- `DB_PASSWORD`: secreto/no versionable.

- `API_TOKEN`: secreto/no versionable.

- `terraform.tfstate`: secreto/no versionable.



### Pregunta 11

Una contraseña o token no debe escribirse directamente dentro de `ci.yml`, `cd.yml` o archivos TypeScript porque quedaría almacenado en el historial del repositorio y podría ser expuesto. Los valores sensibles deben almacenarse utilizando mecanismos como GitHub Secrets.



### Pregunta 12

No. Agregar el archivo a `.gitignore` solamente evita que vuelva a ser agregado en commits futuros, pero el secreto continúa existiendo en el historial de Git.



Si se tratara de un secreto real, se debe revocar o rotar inmediatamente la credencial comprometida y además eliminarla del historial del repositorio cuando corresponda.



### Pregunta 13

`terraform validate` comprueba que la configuración de Terraform sea sintáctica y estructuralmente válida.



`terraform plan` muestra los cambios que Terraform realizaría, pero no los ejecuta.



`terraform apply` ejecuta los cambios definidos por la configuración y crea o modifica los recursos correspondientes.



### Pregunta 14

`ci.yml` se activa mediante `pull_request` porque su objetivo es validar los cambios antes de que sean incorporados a `main`.



`cd.yml` se activa mediante un `push` sobre `main` porque el proceso de entrega continua debe ejecutarse después de que los cambios ya fueron aprobados e integrados en la rama principal.



### Pregunta 15

Terraform automatiza la preparación del ambiente de staging. En este laboratorio utiliza `terraform_data` y un `local-exec` para copiar el frontend construido hacia la carpeta `staging`, simulando una entrega continua dentro del runner de GitHub Actions.



### Pregunta 16

El workflow utiliza `${{ secrets.DEMO_TOKEN }}` porque permite obtener el valor desde GitHub Secrets sin escribirlo directamente en el archivo del workflow ni almacenarlo en el repositorio. Esto permite manejar información sensible de forma más segura.

