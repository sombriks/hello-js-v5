# Parte 3

Precisamos entender como ouvir requisições http com o express e como 
fazê-las com o axios.

## Express.js

- O padrão *de facto* de como cuidar de requisições em diferentes verbos HTTP
- Modular e cheio dos plugins
- Uma *grande quantidade* de projetos derivados
  - Mas se entender o express, entende todos!

### Instalando

```bash
mkdir hello-js-se05-ep03
cd hello-js-se05-ep03
npm init -y
npm install express --save
touch index.js  
```

### Hello World express

```javascript
// index.js
const express = require("express")
const app = express()

app.get("/hello", (req, res) => {
  console.log("hello from the other side!")
  res.send("Hello, world!!!")
})

app.listen(3000)
console.log("server online!")
```

No console chame seu script:

```bash
node index.js
```

Dessa ele não termina, percebe?

- Abra o navegador de internet
- Visite http://localhost:3000/hello

### Parâmetros de url

Modifique o index.js:

```javascript
// index.js
const express = require("express")
const app = express()

app.get("/hello", (req, res) => {
  console.log(req.query)
  res.send("Hello, %s!!!" + req.query.name)
})

app.listen(3000)
console.log("server online!")
```

- Visite http://localhost:3000/hello?name=Joe
- Não aconteceu o que era esperado?
  - Volte no console
  - Mande um CTRL+C
  - Chame o script novamente

### Parâmetros de caminho (path parameters)

Modifique o index.js mais uma vez:

```javascript
// index.js
const express = require("express")
const app = express()

app.get("/hello/:id", (req, res) => {
  console.log(req.params)
  res.send(`Hello, ${req.query.name}!!!, id=${req.params.id}`)
})

app.listen(3000)
console.log("server online!")
```

- Lembre-se de terminar a execução anterior para rodar o script novo
- Agora visitar http://localhost:3000/hello?name=Joe dá erro
- Visite o endereço http://localhost:3000/hello/1?name=Joe
- Ou o endereço http://localhost:3000/hello/2?name=Joe
- Diferente do query, temos certa segurança sobre que variáveis estarão 
  presentes dentro do params.

Além do app.get, tem outros verbos http que o express entende, mas trataremos 
deles no futuro.

## Axios

- Simples e popular
- Dobradinha lá no futuro com o [vue.js](https://vuejs.org/) 

### Instalando

Instale usando o npm:

```bash
# você está na pasta do projeto, certo?
npm install axios --save 
touch index2.js
```

### Uso básico:

```javascript
// index2.js
const axios = require("axios")

const url = "https://api.github.com/search/repositories?q=axios"

axios.get(url).then(ret => console.log(ret.data))
```

- Chame *node index2.js* no console
- Congratulations, você consumiu uma api de serviço!

### Definindo uma baseURL

- Modifique o index2.js

```javascript
// index2.js
const axios = require("axios")

const baseURL = "https://api.github.com"

const api = axios.create({ baseURL })

const params = { q : "axios" }

api.get("/search/repositories", { params }).then(ret => console.log(ret.data))
```

- Mate o processo no console e chame novamente (chato ne? tem solução adiante)
- A documentação completa do axios está [aqui](https://github.com/axios/axios)

## Exercícios de requisição à API's

1. Crie o projeto hello-js-se05-ep03 no seu github e faça o clone dele local
2. Dê init no projeto npm dentro da pasta do repositório
3. Instale no projeto o **express** e o **axios** como dependências. 
   Não esquecer do --save
4. Abra a [documentação da api do github](https://developer.github.com/v3/)
5. Descubra como listar os seguidores de um determinado usuário
6. Crie um script chamado **index.js** e dê require no express e no axios
7. Faça uma rota GET no express que atenda em **/seguidores**
8. Esta rota deverá exibir no navegador o retorno da consulta à API do github
9. Comite o script e faça push para o github. não esquecer do .gitignore
