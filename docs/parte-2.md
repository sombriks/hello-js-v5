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


## Introdução ao npm / package.json

## A linguagem Javascript

## Exercícios básicos de Javascript