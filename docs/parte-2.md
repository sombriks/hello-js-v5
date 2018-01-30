# Parte 2

Precisamos dominar a linguagem e as ferramentas de configuração de projeto.

## Node.js

- Engine de propósito geral derivada do V8, engine javascript dos navegadores
  de internet.
- Programação assíncrona (callback/promise)

### Modo REPL (Read-Eval-Print Loop):

```bash
node
> console.log('hello!')
hello!
undefined
>
```

### Modo script:

```bash
echo "console.log('hello.')" > hello.js
node hello.js
hello.
```
### Passando parâmetros:

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

### Usando variáveis de ambiente:

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

### Definindo e usando um módulo:

```javascript
// lib1.js
exports.foo = x => x * x // i'll explain later!
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
preciso uma ferramenta para organizar o projeto e suas dependências.

### Criando um projeto npm

Projetos em javascript podem ser gerenciados de modo eficiente com o **npm**.

Um projeto npm normalmente possui um arquivo chamado **package.json**

Criar um projeto npm é simples. Abra um terminal e:

(lembrando que VV é o número da edição atual)
```bash
mkdir hello-js-seVV-ep02
cd hello-js-seVV-ep02
npm init -y
```

Por que o npm/package.json é bacana?

- baixar dependências (ex: npm install axios --save)
- definir scripts (ex: npm run dev)
- publicar pacotes no registro público

### Usando módulos

Ao instalar um módulo no seu projeto, ele será guardado numa pasta especial
chamada **node_modules**

Exemplo:

```bash
ls
index.js package.json
npm install crypto-js --save
# várias mensagens do npm depois
ls
index.js node_modules package.json
```

**IMPORTANTE**
```
evite adicionar a node_modules no controle de versão (no git).
```

### Ignorando a node_modules com o .gitignore

Para isso, crie um arquivo chamado **.gitignore** dentro da pasta do
projeto e adicione uma linha contando o nome *node_modules*

```bash
echo node_modules > .gitignore
# ou abrir com um editor o arquivo .gitignore e escrever node_modules
```

### Usando módulos do registro npm

Usar um módulo é parecido com o que vimos anteriormente, exceto que não temos
que nos preocupar com o caminho até o módulo. O node faz a busca por nós:

```javascript
// lib2.js
const CryptoJS = require("crypto-js") // o node acha ele sozinho
module.exports = {
  gibberish (word){
    // https://stackoverflow.com/questions/11889329/word-array-to-string
    return CryptoJS.SHA256(word).toString(CryptoJS.enc.Base64)
  }
}
```

```javascript
// main.js
const gibberish = require("./lib2").gibberish // note o './' dando o caminho
console.log(gibberish("Hello you!"))
// ZmwiiWiOjA8CFPL64+ADb0wl3R2dA+cjZ4SyjJnw3u0=
```

## A linguagem Javascript

- Criada em 1995 por Brendan Eich, então engenheiro na Netscape
- Dinâmica, funcional, baseada em objetos
- Os ponto-e-vírgula são opcionais! :joy: :joy: :joy:

Vamos tratar agora do básico da linguagem. **Apertem os cintos!**

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
- Strings (ex. "azul", 'amarelo', \`verde\`, \`azul ${55}\`)
- Data (ex. new Date())
- Funções (ex. const x = a => 2 + a, function (z) { return z*z } )
- Listas (ex. [], [1,2,3], ["a",'b', new Date(), 4])
- Mapas / Objetos literais (ex. {}, { a: 1, b: "2" })
- Regex (ex. /.* de .* de [0-9]+/ )
- Classes (ex. class Rectangle extends Shape { } )

Aí tem o **null** e tem o **undefined**.
Ambos indicam que a variável não aponta valor. Mais sobre eles adiante.

### Estruturas de controle

```javascript
// switch.
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

// observe que "", 0, null, undefined e false se equivalem nas estruturas de decisão

let a = 0

if(a) console.log("it never happens!")

// for.

for(let k = 0; k < 10; k++) console.log(`eu valho ${k}`)

// while

let i = 10

while(i-->0) console.log("Eu valho %s", i)

// tem o do-while também

let j = 0;
do {
  console.log("eu valho %s", j)
  j = j + 1
} while(j < 10)

```

### Listas

```javascript
// lista vazia
let x = []

// lista com uns valores
let y = [1,2,3]

// adicionar no final da lista
y.push(4)

// adicionar no começo da lista
y.unshift(0)

// remover do final da lista
y.pop() // retorna 4

// remover do começo da lista
y.shift() // retorna 0

// remover de uma posição específica
y = y.splice(1,1) // [1, 3]

// adicionar
y = y.splice(1,0,"x") // [1, "x", 3]

// map, filter, sort
y = y.map(e => "a" + e) // ['a1','ax','a3']
x = [1,2,3,4,5,6,7,8,9]
x = x.filter(e => e > 5) // [6, 7, 8, 9]
x = x.sort((a,b) => b - a ) // [9, 8, 7, 6]

for(let k in x) console.log("%s: %s", k, x)
```

### Mapas

```javascript
let m1 = {} // mapa vazio
m1.x = 2 // {x: 2}

let m2 = { a:1, b:2 }
m2.c = 3 // {a:1,b:2,c:3}

for(let k in m2) console.log("%s: %s", k, x)

// tem isso aqui também. detalhamos adiante.

const api = {
  addr:"https://foo.bar.com",
  list(){
    return [1,2,3]
  },
  key:"XDFGgjhJFffgdg",
}
```

### Funções

```javascript
// clássica, contexto próprio
function a (b, c, d) {
  console.log(c)
  return  b + d
}

// happy arrow, nada de bagunçar contexto
const a = (b, c, d) => {
  console.log(c)
  return  b + d
}

// funções que tenham uma linha usam esse corpo como o prórpio retorno

const b = (c,d) => {
  return c + d
}

const e = (f,g) => f + g

// as duas funções acima se equivalem

// funções com um só parâmetro dispensam os parênteses

const b = c => c * c
b(4) // usando a função
// imprime 16

// funções dentro de mapas tem direito a sintaxe especial:

const obj = {
  myfun(){ return "something" }
}

// tem ainda isso aqui para debatermos:

const x = _ => ({ a:1, b:2 })
```

### Desestruturantes

- Muito úteis, acreditem.

```javascript
let a, b, c = [1,2,3,4]

[a,b] = c // a=1, b=2

let x1 = "cool"
let x2 = "well"
let x3 = { x1, x2 } // {x1:"cool", x2:"well"}

let head = 1
let tail = [2,3,4]
let list = [head, ...tail] // [1,2,3,4] // o "..." é também conhecido como spread

const x = ({a,b,c}) => a * b * c
// equivale a
const y = param => param.a * param.b * param.c

```

### Classes

- Tem que ter, então tem.

```javascript
class Shape {

    constructor (id, x, y) {
        this.id = id
        this.move(x, y)
    }

    move (x, y) {
        this.x = x
        this.y = y
    }
}
```

- Membros privados são na base da amizade
- getter / setter no estilo de uma linguagem acolá ;-)

```javascript
class Point {

  constructor(x,y) {
    this._x = x
    this._y = y
  }

  get coords () {
    return ({
      x:this._x,
      y:this._y
    })
  }

  set coords({x,y}){
    this._x = x
    this._y = y
  }

  get x () { return _x }
  set x (val) {this._x = val }

  get y () { return _x }
  set y (val) {this._x = val }
}
// let p = new Point(1,2)
// p.x // 1
// p.y = 3
// p.coords // {x:1,y:3}
```

- Herança é parecido com a de outras linguagens

```javascript
class Square extends Shape {
  constructor(id,l){
    super(id,l,l)
  }
}
// const sq = new Square("one",1)
// sq.move(2,6)
```

- Imagine o potencial de combinarmos módulos e classes!
- Mais detalhes sobre essas realidades... adiante!

Mais detalhes [aqui](http://es6-features.org)

## Exercícios básicos de Javascript

1. Crie um repositório no github chamado **hello-js-seVV-ep02**
2. Faça o chekout local deste repositório
3. Crie um script chamado **index.js** dentro deste projeto
4. Neste script faça um código que teste se o valor passado por parâmetro para
   o script é par ou ímpar. Se for par, deve imprimir "PAR". Caso contrário,
   deve imprimir "ÌMPAR"
5. Comite este script no *branch* **master**
6. Faça o push deste script para o github
7. Faça um segundo script chamado **index2.js** que calcule uma operação de
   cifra em cima de um argumento recebido dependendo do valor de uma variável
   de ambiente. Se a variável ALG for igual a MD5, use o algoritmo MD5. Se não
   houver a variável ALG, use o algoritmo SHA256 visto no exemplo.
8. Vocẽ deve buscar as formas de uso dos algoritmos na documentação da
   biblioteca [crypto-js](https://github.com/brix/crypto-js)
9. Comite este segundo script também e dê push para o seu github.
10. Se tiver esquecido de ignorar o node_modules não ganha doce! :lollipop:
