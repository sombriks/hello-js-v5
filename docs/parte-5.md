# Parte 5

Precisamos combinar persistência e requisições HTTP numa solução backend.

## Knex migrations

Esquemas de dados não são gravados em mármore. Uma nova coluna pode surgir, 
tabelas podem deixar de existir, novas aparecer.

Em mudando a percepção do negócio, fatalmente o esquema de dados mudará.

Gerenciar a mudança do esquema de dados é missão crítica de qualquer DBA e o 
desenvolvedor fullstack não é exceção.

Entra em cena o conceito de migração de esquema.

### Why migrates?

Uma nova versão do código do aplicativo trará um migrate *caso necessário*.

Com o knex podemos fazer uso de um sistema de migrações. Primeiro nstale o 
knex a nível de sistema:

```bash
sudo npm -g install knex
```

**ATENÇÃO** no windows não tem sudo. *Em teoria* **npm -g install** basta.

Se mesmo assim o windows não disponibilizar o knex na linha de comando, tente o
seguinte:

- Crie um projeto que use o knex:

```bash
mkdir hello-js-seVV-ep05
cd hello-js-seVV-ep05
npm init -y
npm install knex sqlite3 --save
touch index.js
.\node_modules\.bin\knex init
```

Isso cria um arquivo chamado **knexfile.js** com a seguinte estrutura:

```javascript
// Update with your config settings.

module.exports = {

  development: {
    client: 'sqlite3',
    connection: {
      filename: './dev.sqlite3'
    }
  },

  staging: {
    client: 'postgresql',
    connection: {
      database: 'my_db',
      user:     'username',
      password: 'password'
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  },

  production: {
    client: 'postgresql',
    connection: {
      database: 'my_db',
      user:     'username',
      password: 'password'
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: 'knex_migrations'
    }
  }

};
```

É importante que este arquivo esteja na raíz do projeto, como veremos adiante.

 
## Combinando knex, knex migrations e express

## Exercício