# Parte 1

## Git

* Controle de versionamento de código
* Distribuído
* Árvore? Galhos?

Workflow no happy path:

Listando os servidores
```bash
git remote -v
```

Adicionando servidor

```bash
git add origin git@github.com:sombriks/hello-js-v5.git
```
ou
```bash
git add origin https://github.com/sombriks/hello-js-v5.git
```
Tem novidade aí?
```bash
git fetch origin --prune
```
Então vamos trabalhar. Atualizando o branch `master` do servidor para nossa máquina
```bash
git checkout master
git pull origin master
```
e criando o branch de trabalho (local)
```bash
git checkout -b feature/326-create-magic-function
```
Escrevendo código.. :coffee:

Resumo das alterações?
```bash
git status
git diff -w
```
Tudo certo? Vamos commitar (local)
```bash
git commit -a
```
Checando o log
```bash
git log
```
e enviando para o servidor (remoto)
```bash
git push origin feature/326-create-magic-function
```

![gitflow](img/gitflow.svg)

## GitHub

* Open Source / Enterprise

Como saber se um projeto está bem?

* Stars
* Forks

Como contribuir para um projeto?

* Issues
* Pull Requests

E como eu posso me aproveitar disso?

* Portfólio
* GH Pages (`you`.github.io)

## Consoles (cliente | servidor)

Aqui no navegador (`ctrl+shift+ i`)

```javascript
>
console.log('hello, js!')
```

No terminal, NodeJS

```bash
node
>
```

```javascript
console.log('hello, js!')
```

É browser?

```bash
npm -version
node -version
```

É V8!

Uma das *várias engines javascript,

