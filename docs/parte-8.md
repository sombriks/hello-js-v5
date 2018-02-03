# Parte 8

- Precisamos ter todas as técnicas vistas engatilhadas
- Precisamos de um pouquinho de teoria
- Precisamos de um pouco de reuso 
- Precisamos entender o cliente (o ser humano!)
- Precisamos adestrar nosso cliente
- Precisamos fazer aquele projetão que você respeita

## Conceitos de desenvolvimento fullstack

Por exemplo, você é desenvolvedor fullstack agora.

`Mas o que é uma stack?`

*Stack*, ou pilha, é esse monte de tecnologias que você conhece agora.

**Parabéns**

Existem várias stacks. 

Não necessariamente uma *"pilha completa"* envolve tecnologias web.

O dev fullstack é capaz de entregar uma solução completa para um problema 
existente.

É muito comum o fullstack resolver a trindade *front-back-banco*.

## Boas práticas em projetos Javascript

*Papo meio esteticista*

### Os nomes das variáveis e funções

```javascript
// OK
const valoratual = x
const novovalor = x * 2
```

```javascript
// OK
const valorAtual = x
const novoValor = x * 2
```

```javascript
// OK
const valor_atual = x
const novo_valor = x * 2
```

```javascript
// DON'T
const valor_atual = x
const novoValor = x * 2
```

- mantenha a coesão de estilo
- decida no começo do projeto 
- e mantenha o que já estiver

### Os nomes das classes

- ClasseNomeieComCamelCase

### Os nomes dos arquivos

- os-arquivos-nomeie-assim.js (GOOD)
- f0003-listagem-de-pedidos.vue (BETTER)

### As pastas

- agrupe as funcionalidades (features)
- agrupe os componentes reutilizáveis (components)

### Documentação básica

- **README.md** com 
  - contato do(s) autor(es)
  - requisitos de sistema
  - compatibilidade entre plataformas
  - instruções básicas de execução

### Mais de um projeto no sistema

- coloque-os no mesmo repositório
  - coesão entre as versões
  - vende a ideia correta de conjunto

### Os exports são no fim do arquivo!

- em seus módulos (arquivos), os exports devem ser no fim do arquivo
- evitar referências cíclicas imediatas

```javascript
// src/components/module-a.js

const foo = _ => "foo"

const bar = require("./module-b").bar

const vish = _ => bar()

exports.foo = foo
exports.vish = vish

// src/components/module-b.js

const foo = require("./module-a").foo() // VERY BAD

const bar = _ => console.log("bar: %s", foo)

exports.bar = bar

// src/main.js

const m1 = require("./components/module-a")

m1.vish() // erro
```
- saída:

```bash
[sombriks@ramza s03e08]$ node main.js
/home/sombriks/Documentos/s03e08/components/module-b.js:3
const foo = require("./module-a").foo() // BAD
                                  ^

TypeError: require(...).foo is not a function
    at Object.<anonymous> (/home/sombriks/Documentos/s03e08/components/module-b.js:3:35)
    at Module._compile (module.js:570:32)
    at Object.Module._extensions..js (module.js:579:10)
    at Module.load (module.js:487:32)
    at tryModuleLoad (module.js:446:12)
    at Function.Module._load (module.js:438:3)
    at Module.require (module.js:497:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (/home/sombriks/Documentos/s03e08/components/module-a.js:3:34)
    at Module._compile (module.js:570:32)
```

- não é que foo não seja uma função, mas *na hora* que o modulo-b faz a busca por foo, ele ainda não foi exportado.
- para evitar problemas assim:

```javascript
// src/components/module-b.js

const bar = _ => console.log("bar: %s", require("./module-a").foo()) // WORKS

exports.bar = bar
```

### Abstract DAO (yaaay!!!)

```javascript
// src/components/base-dao.js

const knex = require("./config").knex

class AbstractDao {

  constructor(table, idcolumn) {
    this.idcolumn = idcolumn
    this.table = table
  }

  list(q, p=1, s=10) {
    return knex(this.table).select().where(q).limit(p).offset((p-1)*s)
  }

  find(id) {
    return knex(this.table).select().where(this.idcolumn,id)
  }

  insert(data) {
    return knex(this.table).insert(data,this.idcolumn)
  }

  update(data) {
    return knex(this.table).update(data).where(this.idcolumn, data[this.idcolumn])
  }

  del(id) {
    return knex(this.table).del().where(this.idcolumn, data[this.idcolumn])
  }

}

exports.AbstractDao = AbstractDao
```

- tem quem goste muito
- fácil de acoplar num **express.Router()** 
- através da herança é possível definir outras chamadas especiais de busca

### Testes

```bash
npm install mocha chai --save-dev
mkdir tests
touch tests/f0002-pedidos.js
```

- [mocha](https://mochajs.org/)
- [chai](http://chaijs.com/)

```javascript
// tests/f0002-pedidos.js
const chai = require('chai');
const pedidos = require("../src/features/pedidos")

chai.should() // http://chaijs.com/guide/styles/#should

describe("Teste de pedidos", _ => {

  let idpedido

  it("Deveria criar um novo pedido", done => {
    pedidos.insert({
      dscpedido:"10 biscoitos bono"
    }).then(ret => {
      ret.should.have.property("idpedido")
      idpedido = ret[0]
      done()
    }).catch(err => {
      done(err)
    })
  })

  // ... 

})
```


## Padrões de projeto em Javascript

### App profile

- Seu sistema se comporta diferente dependendo das variáveis de ambiente
- **cross-env**
- *myapp.service*

Se você instalar, seja no front ou no back, o cross-env, aquele problema chato
dos windows teimarem com as variáveis de vez em quando é resolvido:

```bash
npm install cross-env --save-dev
# se for um font com browserify, instale também:
npm install envify --save-dev
```

Aí na seção de scripts dos **package.json**:

```json
{
  // ...
  "scripts":{
    "dev":"cross-env NODE_ENV=development nodemon src/main.js"
  },
  // ...
}
```

Ou:

```json
{
  // ...
  "scripts":{
    "dev":"cross-env NODE_ENV=development budo src/main.js:build.js -o -l",
    "build":"cross-env NODE_ENV=production browserify src/main.js -o build.js" 
  },
  // ...
  "browserify":{
    "transform":[
      "envify"
    ]
  }
}
```

O envify é necessário porque, como sabemos, browser não vai dar variável de 
ambiente para os nossos scripts. Mas o browserify, na hora que rodar, vai ter
direito a variáveis de ambiente. Aí dá samba.

Exemplo do NODE_ENV sendo útil no lado do serviço:

```javascript
// src/config.js
const cfg = require("../knexfile")
let env = process.env.NODE_ENV || "development" // failsafe
const knex = require("knex")(cfg[env])
exports.knex = knex
exports.env = env // só pra conferir em outros lugares
```

Exemplo de NODE_ENV sendo útil no lado do cliente:

```javascript
// s05ep08-client/src/api.js
const axios = require("axios")

let env = process.env.NODE_ENV || "development" // failsafe

const serviceaddr = {
  development:"http://127.0.0.1:3000",
  staging:"https://homolog-myawesomeservice.com",
  production:"https://myawesomeservice.com",
}

const api = axios.create({
  baseURL: serviceaddr[env]
})

exports.festa = {
  list: query => api.get("/festa/list", { params:query }),
  find: idfesta => api.get(`/festa/${idfesta}`),
  save: data => api[data.idfesta?"put":"post"]("/festa/save", data),
  del: idfesta => api.delete(`/festa/${idfesta}`)
}
// convidados, convites, etc
```

Ou ainda, na hora de criar um 
[serviço do systemd](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units) 
pra subir com aquela droplet
contratada lá na digital ocean:

```conf
[Service]
Environment=NODE_ENV=production
ExecStart=/usr/bin/node /opt/hellojs-s0e5-ep07/s05ep08-service/src/main.js
WorkingDirectory=/opt/hellojs-se05-ep07/se05ep08-service
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=s03e07
User=hellouser
Group=hellouser

[Install]
WantedBy=multi-user.target
```

### Design patterns clássicos

Você pode fazer uso dos padrões clássicos 
([GoF](https://pt.wikipedia.org/wiki/Padr%C3%A3o_de_projeto_de_software)) e 
também pode fazer uso dos 
[paradigmas funcionais](https://en.wikipedia.org/wiki/Functional_programming).
A grande questão é quando e por que adotar um.

Por exemplo, podemos usar, como foi visto lá no exemplo de boas práticas, um 
[DAO](https://en.wikipedia.org/wiki/Data_access_object). Mas é possível 
açcançar o mesmo feito com uma abordagem puramente funcional.

Outro exemplo: Um Singleton não dá pra fazer com as classes do javascript, 
pois elas não tem construtor privado, mas o módulo se comporta como um 
singleton. Ele só é definido uma vez e pronto.

Adapter não faz nem sentido em linguagens dinâmicas, onde a 
[Liskov Substitution](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
Precisa de nenhum contrato, nenhum formalismo. Duck Typing total.

Injeção de dependência também sofre de relevância, posto que os módulos sobem 
na memória uma vez e uma vez apenas. A gestão das instãncias não é problema 
grave como seria em outras linguagens e as questões de concorrência são 
mitigadas pela ausência de memória compartilhada. Isso mesmo meus amigos, 
temos green threads (ou fake threads. ou 'tenho 8 cores pra quê threads'. 
Your call).

Algumas coisas, entretanto, são eternas. 
[MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) é 
aquela lei imutável. Note como as rotas do express cuidam apenas de controle, 
praticamente. 

Lembrem-se do **importante**: Não aplicamos design patterns por serem bonitos.
Usamos patterns porque surge uma situação onde aaplicação do pattern correto
facilitará nossa vida no futuro: **escrever menos, alterar menos**.

## Levantamento de requisitos

Tudo isso é muito bacana, mas nem sempre o problema chega mastigado esperando 
apenas a implementação da solução.

É possível que, na condição de desenvolvedor full-stack, você precise estudar
o problema a ser resolvido *na fonte*. 

**Ter que falar com gente. D:**

### Como descobrir o que fazer

- Entender o problema existente via muita conversa e anotação
- [Brainstorm](https://en.wikipedia.org/wiki/Brainstorming)
- [Mapa mental](https://en.wikipedia.org/wiki/Mind_map)
- Qualquer abordagem deve resultar numa lista de coisas que o novo sistema deve fazer
  - ex: brainstorm do [red-line-finance](https://github.com/sombriks/red-line-finance/blob/master/README.md#feature-storm)

### Quanto tempo leva?

- De posse da lista de coisas que o sistema deve fazer
  - Agrupar em funcionalidades
  - Numerar essas funcionalidades
  - Contar quantas telas cada funcionalidade terá
  - Contar quantas entidades as funcionalidades demandam
    - Ex: comprar sanduíche online (funcionalidade)
      - Tela pra listar sanduíches
      - Tela do carrinho de compras
      - Tela de últimos pedidos
      - Entidade sanduíche
      - Entidade pedido
    - **DICA GOLD**
      - Crie um código para cada feature definida aqui
      - Bote esse código no nome dos artefatos de software que implementarem 
        essa feature
        - **Rastreio bidirecional entre requisito e código**
- Em cima das telas e das entidades, medir o tempo
  - *"Ahhh mas comprar online não sei dizer o tempo"*
    - Tela de lista não sei dizer o tempo...?
    - Fazer uma tabela... fazer uma tabela eu sei
    - Criar um arquivo com seção tempate e script eu sei
    - Colocar um campo no formulário eu sei
  - Quanto mais você detalhar uma atividade, mais fácil de medir o tempo
  - Dê um tempo para cada detalhe, a soma dará uma boa ideia do tempo total
  - Jogue uns 30% de tempo como "risco"
  - Conte as horas e multiplique pelo valor bruto (vc + impostos + margem) das 
    suas horas


### Requisitos não-funcionais

Algumas atividades não são diretamente ligadas às necessidades dos clientes.

Ainda assim, elas demandam tempo e são predecessoras de outras coisas.

Exemplo: é preciso definir a estrutura dos projetos (front, back, mobile, 
crawler, etc) e preparar o repositório. É preciso contratar servidor e os 
nomes DNS.

Requisitos não-funcionais devem entrar nos planejamentos e as horas que sejam
estimadas para a realização deles devem ser cobradas, bem como os custos 
recorrentes colocados na ponta do lápis.

## Ferramentas de acompanhamento de projeto

- [Trello](https://trello.com/)
  - Simples
  - Fácil
  - Meio chato pra fazer relatórios (tem que fazer 'namão' porque não tem)
- [Pivotal Tracker](https://www.pivotaltracker.com/pricing)
  - Boa ferramenta agile
  - Depois de um dia mexendo fica sussa
  - Cálculo automático de sprints :+1: 
- [Project Libre](https://sourceforge.net/projects/projectlibre/files/)
  - Bom pra montar gantt (primeiro orçamento)
  - Alimenta o calendário com os feriados e ele calcula só a data estimada de fim
  - Coloca um recurso e um custo e ele diz quanto que vai custar
  - Cliente gosta de saber quanto vai gastar, mesmo que esteja errado
  - Dá pra fazer proposta com o Project e depois criar as sprints no Pivotal
  - Tem relatórios, baseline cost, etc

## Exercício vue+SFC+SPA com backend express+knex

Hora da verdade.

![megaman](img/megaman.webp)


### Especificação

O projeto Hello-JS usa um método muito interessante de listar a presença dos
participantes. Para cada episódio, uma issue no github do projeto é criada.

A regra de nomes dessas issues do github é bem definida: A primeira parte, por 
exemplo, se chama **SE05EP01**.

A segunda parte e chama **SE05EP02**, a terceira **SE05EP03** e assim segue até
a oitava e última parte, **SE05EP08**.

Usando todas as técnicas vistas até agora:

1. **Especifique** um sistema que faça uma consulta à api do github e recupere 
   a quantidade de presenças de um determinado participante
2. O sistema terá uma tela onde é informado o apelido de github do provável
   participante e um botão de verificação
3. Ao verificar as 8 issues, devemos ter um serviço backend que salvará a 
   quantidade de comentários deste participante em cada uma das issues
4. Uma segunda tela do sistema deverá listar todos os participantes e as 
   presenças em cada parte do hello-js. Esta segunda tela não deve consultar 
   uma api remota, mas sim o backend local
5. A especificação deve conter, no mínimo, um brainstorm e umas propostas de
   interface de usuário
6. A navegação entre as duas telas deve ser garantida com o vue-router
7. Use os componentes do vue-material para dar melhor aspecto às telas

### Conclusão

O profissional FullStack não é um faz-tudo. Antes disso, ele é um profissional
versátil, capaz de não apenas atuar em diversas posições, mas também liderar.

Nas situações onde é preciso atuar com uma equipe reduzida, ele deve buscar na
ampla seara de tecnologias que domina o melhor caminho para entregar uma 
solução de qualidade e de bom custo/benefício.

[Faça boa arte](https://www.youtube.com/embed/nS-u4MBGSFs)
