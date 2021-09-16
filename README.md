# Javascript-lab17

## Módulo 6: Herramientas del ecosistema Javascript
Conocer y manejar a nivel usuario las herramientas complementarias de Javascript.

### Unidad 3: Linters y Husky

Configuración de repositorios con heramientas para análisis estáticos (code quality), Husky como herramienta para gestionar el workflow de desarrollo			

### Instalación del proyecto

- npm install
- npm test

- npx sequelize db:migrate
- npx sequelize db:seed:all
- npm run dev

Probar http://localhost:3000/api/v1/artists


#### 👀 Instrucciones 👀 

#### ES Lint con Standard

###### ¿Qué es Standard?
https://standardjs.com/

###### ¿Quién lo usa?
https://standardjs.com/#who-uses-javascript-standard-style

###### ¿Como se relaciona a Eslint?
https://github.com/standard/eslint-config-standard
Ejecutaremos el comando
  `npm install -D eslint-config-standard`
una vez ejecutado 

`npx eslint --init`

##### How would you like to use ESLint?
  > To check syntax, find problems, and enforce code style

##### What type of modules does your project use?
  > CommonJS (require/exports)

##### Which framework does your project use?
  > None of these

##### Does your project use TypeScript?
  > No 

##### Where does your code run? (utilizar la barra espaciadora)
  > ✔ Browser
  > ✔ Node

##### How would you like to define a style for your project? … 
  > Use a popular style guide --> Standard

##### Which style guide do you want to follow
  > Standard: https://github.com/standard/standard

##### What format do you want your config file to be in?
  > JSON

Esto ultimo puede ser en múltiples formatos. Ver más ejemplos en este [link](https://eslint.org/docs/user-guide/configuring/configuration-files#configuration-file-formats)

Por último responderemos que usaremos npm

##### Would you like to install them now with npm?
> Yes


Agregar al `package.json` un script llamado "lint" y ahora vamos a ejecutar

```
  npm run lint
```

Veremos varios errores y solucionaremos algunos para revisar las reglas.

Ahora probemos `npx eslint --fix src`

Y dejamos pasando el linter.


#### Npm Audit

npm audit
npm audit fix

--- ejemplo manual --

ver la más actual
`npm view tar version`

ver la version instalada
`npm ls tar`

`npm shrinkwrap`

seguiremos el siguiente modelo apoyados de `npm ls` y del siguiente modelo:

```json
{
    "dependencies": {
        "A": {
            "version": "actual-version",
            "dependencies": {
                "B": { 
                  "version": "actual-version",
                  "dependencies": {
                    "c": {
                      "version": "PATCH-VERSION"
                    }
                  }
                }
            }
        }
    }
}
```

modificaremos a mano shrinkwrap

  `buscar paquete afectado y cambiar version una major hacia arriba`
  `rm -rf node_modules`
  `npm ls <package>`
  `npm test`
  `npm run dev`

Probamos hasta estar seguros que funciona.

Por última vez nos aseguramos que ya no hay vulnerabilidades con el comando de audit:

```
  npm audit
````

#### ¿Qué otras cosas podemos hacer?
Más información de todo lo que podemos lograr en este [link](https://blog.logrocket.com/static-analysis-in-javascript-11-tools-to-help-you-catch-errors-before-users-do/#:~:text=Static%20analysis%20is%20the%20process,web%20server%20or%20build%20process)


### Instalar husky
```bash
npm i -D husky
npx husky install
```

Veras que se configuro una nueva carpeta en la raíz llamada `.husky`

Ahora ejecutaremos lo siguiente:

```bash
npx husky add .husky/pre-commit "npm run lint"
npx husky add .husky/pre-push "npm test"
```

ahora veremos que dentro del directorio llamado `.husky` hay 2 nuevos archivos. Si revisamos su interior deberíamos ver lo siguiente:

**.husky/pre-commit**

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint

```

**.husky/pre-push**

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm test

```

Ahora podemos probar que todo esta funcionando.
Primero haremos un commit y veremos como es que se corren los linters de los proyectos Frontend y Backend.

```bash
git add .
git commit -m "chore(dev-scripts): adding pre-commit and pre-push scripts"
```

Veremos como al hacer esto se corre el script `pre-commit`. Si todo salió bien el commit debería hacerse sin problemas.
### Conventional Commits y Changelog

##### ¿Qué es conventional commits?

https://www.conventionalcommits.org/en/v1.0.0/

```
npx husky add .husky/prepare-commit-msg "exec < /dev/tty && node_modules/.bin/cz --hook || true"
```

Ahora al hacer los commandos de git veremos un CLI que nos hará una serie de preguntas:

```
git add .
git commit
```

##### ¿Crear un Changelog?

Ejecutaremos
```
npm i -D conventional-changelog-cli
npx conventional-changelog -p angular -i CHANGELOG.md -s
```

y agregamos el script en el `package.json`
```
  "version": "conventional-changelog -p angular -i CHANGELOG.md -s"
```
y ahora luego de agregar los archivos y realizar el proceso ejecutaremos

```
npm version patch
```

y ya tenemos un completo ambiente de desarrollo profesional.