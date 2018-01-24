# Parte 2

Precisamos dominar a linguagem e as ferramentas de configuração de projeto.

## Node.js

- Engine de propósito geral derivada do V8, engine javascript dos navegadores
  de internet.
- Programação assíncrona (callback/promise)

Modo REPL (Read-Eval-Print Loop):

```bash
node
> console.log('hello!')
hello!
undefined
>
```

Modo script:

```bash
echo "console.log('hello.')" > hello.js
node hello.js
hello.
```
Passando parâmetros:

```javascript
// parameters.js

if(process.argv.length < 3){
  console.log('usage: node parameters.js joe')
  return
}
console.log("Hello, %s!",process.argv[2])
```

Daí:

```bash
node parameters.js Yuri
Hello, Yuri!
```

Usando variáveis de ambiente:

```javascript
// envparams.js

if(!process.env.FOO)
  console.log('FOO variable is not set')

console.log("FOO: %s, BAR: %s!",process.env.FOO,process.env.BAR)
```

Daí:

```bash
FOO=hello BAR=world node parameters.js Yuri
FOO: hello, BAR: world!
```

Nota: no windows dá problema definir variáveis assim, mas solucionamos isso num
futuro próximo.

Definindo e usando um módulo:

```javascript
// lib1.js
exports.foo = x => x * x
```

```javascript
// main.js
const foo = require("./lib1").foo
console.log(foo(4))
```

- Os dois arquivos precisam estar na mesma pasta
- É "./lib1" mesmo, não é "./lib1.js"

A saída deixo vocês adivinharem ;-) 

## Introdução ao npm / package.json

O segredo de entregar uma solução bem-sucedia e de boa manutenção é ter um bom
processo. Organização. Procedimentos de trabalho.

Além do git, que nos salva na hora de registrar as evoluções da solução, é 
preciso uma ferramenta para organizar o projeto e suasdependências.

Projetos em javascript podem ser gerenciados de modo eficiente com o **npm**.

Um projeto npm normalmente possui um arquivo chamado **package.json**

Criar um projeto npm é simples. Abra um terminal e:

```bash
mkdir hello-js-se05-ep02
cd hello-js-se05-ep02
npm init -y
```

Por que o npm/package.json é bacana?

- baixar dependências (ex: npm install axios --save)
- definir scripts (ex: npm run dev)
- publicar pacotes no registro público

## A linguagem Javascript

- Criada em 1995 por Brendan Eich, então engenheiro na Netscape
- Dinâmica, funcional, baseada em objetos

### Variáveis

Nada revolucionário, vocês já devem conhecer:

```javascript
a = 2 // crusty but works
var b = 3 // old fashined and scope concerns
let c = 4 // nice, scoped, fair
const d = 5 // the functional way
```

Nota: as variáveis 'a' e 'b' ficam no escopo global ou no escopo da função. 
As variáveis c e d vão para o escopo do bloco sempre, seja bloco de função 
ou bloco de controle.

### Tipos de dados

- Números (ex. 2, 1.5, 3000)
- Strings (ex. "azul", 'amarelo', `verde`)
- Data (ex. new Date())
- Funções (ex. const x = a => 2 + a, function (z) { return z*z } )
- Listas (ex. [], [1,2,3], ["a",'b', new Date(), 4])
- Mapas (ex. {}, { a: 1, b: "2" })
- Regex (ex. /.* de .* de [0-9]+/ )
- Classes (ex. class Rectangle extends Shape { } )

### Estruturas de controle

```javascript
// switch. evite. 
switch(x){
  case 1:
    console.log("got one");
    break;
  case "deer":
    console.log("got baaaa!!!");
    break;
  default:
    return "end!";
}

// if.
if(a > 1) console.log("we have a big number")

if(b == 3) {
  console.log("Too many instructions")
  console.log("Too long words")
}

// 

```

### Funções

```javascript

```

### Listas

```javascript

```

### Mapas

```javascript

```

### Desestruturantes 

```javascript

```

### Classes

```javascript

```

### Herança

```javascript

```

Mais detalhes [aqui](http://es6-features.org)

## Exercícios básicos de Javascript

1. Crie um repositório no github chamado **hello-js-se05-ep02**
2. Faça o chekout local deste repositório
