# Parte 7

- Precisamos diferir projeto de repositório, pois cada repo poderá conter mais 
  de um projeto
- Precisamos automatizar o recarregamento dos scripts e assets
- Precisamos separar, a nível de projeto, cliente de serviço/servidor
- Precisamos de um toolkit de alto nível que resolva o CSS da aplicação cliente
- Precisamos nos aprofundar no vue e dominar o conceito de componente 

## Browserify

- É um bundler, um "empacotador" de javascript para o lado do cliente
- Permite usar o exports/require como se estivéssemos no node
- Livra seu código dos problemas de namespace global que as tags de script 
  sofrem ao tratar cada arquivo js como um módulo isolado
- Tenta disponibilizar as bibliotecas server side para o client side, quando
  compatíveis.

### Instalando

```bash
sudo npm -g install browserify
```

Se for um windows ou um linux onde vocẽ não tem poderes administrativos, use a 
estratégia de instalar como dependência de projeto e use a partir da pasta .bin
dentro de node_modules (como foi feito com o knex):

```bash
mkdir sample
cd sample
npm init -y
npm install browserify --save-dev
```

O **package.json** deve ficar mais ou menos assim:

```json
{
  "name": "sample",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "browserify": "^15.2.0"
  }
}
```

Nota: o browserify só é necessário em tempo de desenvolvimento. O arquivo 
resultante não depende dele. Por isso ele é salvo com **--save-dev**

### Uso básico

Teremos agora uma pasta **src** e dentro dela os módulos do projeto:

```bash
# na pasta sample
mkdir src
cd src
touch main.js
touch mymodule1.js
touch anothermodule.js
```

A estrutura deve se parecer com essa:

```bash
sample
├── node_modules
├── package.json
├── package-lock.json
└── src
    ├── anothermodule.js
    ├── main.js
    └── mymodule1.js
```

Nossas bibliotecas hipotéticas são módulos exportando alguma coisa, conforme 
vimosanteriormente:

```javascript
// anothermodule.js

```

```javascript
// mymodule1.js

```

O main.js faz as vezes de "ponto de entrada" (i.e. aquele script que era 
passado para o node. *mas aqui não tem node*):

```javascript
// main.js
const foo = require("./mymodule1")
const bar = require("./anothermodule").bar

foo(2)
console.log(bar(3))

```

## Exercício de build com browserify

## Projeto fullstack com browserify

## Budo e nodemon

## vue-material

## Exercício protótipo fullstack

## Projeto client-side com SPA (single page application)

## Render function e single file components