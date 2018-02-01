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
exports.bar = x => x * x * x
```

```javascript
// mymodule1.js
module.exports = x => console.log(x * x)
```

O main.js faz as vezes de "ponto de entrada" (i.e. aquele script que era 
passado para o node. *Mas aqui não tem node*):

```javascript
// main.js
const foo = require("./mymodule1")
const bar = require("./anothermodule").bar

foo(2)
console.log(bar(3))
```

Agora é só voltar pro console e usar o browserify para "empacotar" tudo:

```bash
browserify src/main.js -o build.js
```

Lembrando que, no caso do windows, o browserify está em
**.\node_modules\.bin\browserify**

## Exercício de build com browserify

1. Crie pelo **github** o repositório **hello-js-seVV-ep07**
2. Faça checkout
3. Dentro da pasta do repositório, crie uma pasta chamada **se05ep07-client**
4. Entre nesta pasta e dê **npm init -y** nela. 
5. Crie um **index.html** dentro de *se05ep07-client* com a seguinte estrutura:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Hello browserify!</title>
</head>
<body>
  <div id="mountpoint"></div>
  <script src="build.js"></script>
</body>
</html>
```
6. Crie uma pasta **src** e dentro dela crie um arquivo chamado **main.js**
7. Modifique no **package.json** a sessão de scripts. Deve ficar mais ou menos assim:
```json
{
  "name": "se05ep07-client",
  "version": "1.0.0",
  "description": "",
  "main": "build.js",
  "scripts": {
    "build": "browserify src/main.js -o build.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "browserify": "^15.2.0"
  }
}
```
8. Experimente fazer o build do projeto através do script criado:
```bash
npm run build
```

## Projeto fullstack com browserify

Daqui por diante, **a raíz do repositório não é mais a raíz do projeto**.

O repositório contará com dois projetos distintos: um "-client" e um "-service".

Esta separação é benéfica, pois embora seja possível usar a pasta static do 
express para servir recursos para o navegador, a ideia de um projeto dentro do
outro deve ser evitada enquanto possível.

Adicionalmente, se usássemos o npm dentro da pasta static, teríamos problemas 
para não se perder entre os artefatos. *Onde é servidor? Onde é cliente?*

Mantendo os dois projetos, frontend e backend, dentro do mesmo repositório 
git, entretanto, sugere para quem baixa o repositório que, em teoria, aquela 
versão do cliente é compatível com aquela versão do serviço.

## Budo e nodemon

Um dos passos mais aborrecidos no dia a dia de um desenvolvedor é testar as 
alterações de código ao vivo. 

O ciclo alterar - recarregar - visualizar é responsável, muitas vezes, por 
30% do tempo gasto escrevendo uma nova feature. 

Há tecnologias, inclusive, que os servidores levam até 3 minutos para mostrar 
o resultado da alteração.

No node, felizmente, levamos apenas alguns segundos, mas queremos melhor, 
queremos mais.

### Seção scripts do package.json

Antes de usarmos os reloaders, vamos tratar de uma técnica essencial para isso
tudo funcionar.

Vimos no exercício anterior um jeito de usar o package.json para guardar o 
comando de empacotamento do browserify.

A seção de scripts serve para rodar comandos. A diferença é que os comandos 
dentro da seção de scripts tem direito àquela pasta **.bin** que fica 
escondida dentro da **node_modules**.

Por exemplo, se eu quiser colocar a chamada do **knex** num **package.json**,
ficaria assim:

```json
{
  "name": "se05ep07-service",
  "version": "1.0.0",
  "description": "",
  "main": "src/main.js",
  "scripts": {
    "knex": "knex",
    "dev": "nodemon src/main.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "body-parser": "^1.18.2",
    "cors": "^2.8.4",
    "express": "^4.16.2",
    "knex": "^0.14.2",
    "morgan": "^1.9.0",
    "sqlite3": "^3.1.13"
  },
  "devDependencies": {
    "nodemon": "^1.14.11"
  }
}
```

E o uso deste script npm ficaria assim:

```bash
npm run knex -- migrate:make esquema_inicial
```

Como o knex depende de argumentos para funcionar corretamente, é preciso 
passar um "--" (dois traços) para indicar que os argumentos seguintes são 
para o programa que o script invoca.

Outro ponto muito valioso a ser indicado é que o package.json é um arquivo no
formato [JSON](https://www.json.org) no modo mais estrito possível. Não dá 
inserir comentários e nem colocar vírgulas demais ou de menos. Chaves precisam
ser strings válidas.

### Exercício

1. Na pasta do repositório **hello-js-seVV-ep07** crie uma pasta 
   chamada **se05ep07-service**
2. Entre nessa pasta e dê npm init nela.
3. Instale as dependências server-side **sqlite knex express body-parser cors**
   **morgan** e não esqueça do --save
4. Instale uma dependência chamada **nodemon** e salve com **--save-dev** 
5. modifique o package.json para ter na seção de scripts o knex e o nodemon
   conforme visto no exemplo anterior
6. Crie uma pasta chamada **src** e dentro dela um arquivo chamado **main.js**
7. Dê um **npm run knex -- init** na raíz do **projeto** service. 
   Não confundir com a raís do **repositório**

Daqui por diante, quando quisermos rodar um projeto javascript, sempre 
digitaremos no console um **npm run dev** e vamos esperar que tudo funcione :-)

### nodemon

O [nodemon](https://nodemon.io/) vigia alterações nos scripts e recarrega 
sozinho o seu serviço.

Em vez de alterar o script, voltar ao console, matar o script e chamar 
novamente, chame o nodemon uma vez que ele fica recarregando os scripts 
por você de modo transparente.

Lembre-se, entretanto, de matar o nodemon quando estiver criando arquivos de
migração. Se vocẽ criar um arquivo de migração enquanto o nodemon estiver em
operação, e se o seu serviço tiver aquele gancho pra rodar as migrações 
automaticamente, ele tentará rodar migrações vazias no seu banco, e isso é um 
grande aborrecimento.

### budo

Entre no projeto cliente e instale a dependência de desenvolvimento abaixo:

```bash
cd se05ep07-client
npm install budo --save-dev
```

Em seguida, modifique seu package.json para conter o seguinte script:

```json
{
  "name": "se05ep07-client",
  "version": "1.0.0",
  "description": "",
  "main": "build.js",
  "scripts": {
    "build": "browserify src/main.js -o build.js",
    "dev": "budo src/main.js:build.js -o -l"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "browserify": "^15.2.0",
    "budo": "^11.0.1"
  }
}
```

De maneira análoga ao que

## vue-material

## Exercício protótipo fullstack

## Projeto client-side com SPA (single page application)

## Render function e single file components