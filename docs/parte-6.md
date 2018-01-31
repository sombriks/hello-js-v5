# Parte 6

Precisamos recepcionar os dados que chegam do servidor usando javascript para 
compor dinamicamente a tela para apresentarmos os dados. Usaremos frameworks
de vanguarda para simplificarmos o processo de aquisição e apresentação dos 
dados recuperados e também enviaremos dados para o backend usando estas mesmas
tecnologias front-end.

## Javascript no navegador de internet

Após 20 horas rodando javascript no lado do servidor, vamos voltar para onde a 
linguagem nasceu.

Antes de rodar javascript no navegador, você precisa de uma tag de script no 
seu documento HTML:

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      // index.html
      console.log("Hello world!!!")
    </script>
  </body>
</html>
```

Ao abrir este arquivo no navegador, o documento estará vazio. Mas se você inspecionar a página (`ctrl+shift+i`):

![console-javascript-browser](img/console-javascript-browser.png)

Observe que este inspetor oferece muitas outras coisas além de um console, mas trataremos delas adiante.

O javascript no browser está *embutido* (*embedded*), portanto não tem como 
usar diretamente argumentos ou variáveis de ambiente nele.

### Manipulando a DOM

O javascript no browser pode, entretanto, manipular os elementos do documento 
HTML. Modifique o index.html para exemplificarmos isso:

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      // index.html
      let i = 100
      while(i--) {
        const p = document.createElement("p")
        p.innerHTML=i
        document.body.appendChild(p)
      }
    </script>
  </body>
</html>
```

Nunca foi tão fácil fazer um documento de 99 parágrafos, ;-)

Assim como temos o process global no node, no javascript também temos alguns 
objetos globais. o **document** é um deles. Ele nos dá acesso à àrvoce de 
elementos do documento.

### Bibliotecas javascript através da internet

Assim como vimos no node, podemos usar bibliotecas no lado do navegador também.
Para tanto, basta incluirmos uma tag de script com o atributo **src** 
apontando para a biblioteca que desjamos usar.

Exemplo:

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="catlist">
      <!-- dynamic content goes here -->
    </ul>
    <!-- https://cdnjs.com/libraries/axios -->
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.17.1/axios.js"></script>
    <script>
      // index.html
      const api = axios.create({
        baseURL:"https://api.github.com"
      })
      // https://developer.github.com/v3/issues/#issues
      api.get("/repos/sombriks/hello-js-v5/issues").then(ret => {
        console.log(ret)
        ret.data.map(e => {
          const li = document.createElement("li")
          li.innerHTML = `<a href="${e.url}">${e.title}</a>`
          document.getElementById("catlist").appendChild(li)
        }).catch(err => {
          console.log(err)
          const li = document.createElement("li")
          li.innerHTML = "erro!"
          document.getElementById("catlist").appendChild(li)
        })
      })
    </script>
  </body>
</html>
```

Note que o uso do axios é o mesmo no browser e no servidor. Nem toda biblioteca 
javascript é assim, mas é um bônus muito bem vindo sempre.

Se precisamos recuperar uma tag para usar no script, **document.getElementById** 
é a função para a missão. A tag precisa, claro, ter um atributo **id** com um 
valor único (que não se repita) no documento.

A tag de script que nos fornece o axios está num servidor externo. Esse tipo de 
servidor, que fornece scripts e estilos css, chamamos de **CDN**, Content 
Delivery Network.

Usar CDN's é vantajoso porque você pode testar muito rápido determinada solução ou experimentar uma biblioteca. Abaixo alguns serviços de CDN:

- [cdnjs](https://cdnjs.com)
- [jsdelivr](https://www.jsdelivr.com)
- [unpkg](https://unpkg.com)

A desvantagem é que sua aplicação fica dependente de um serviço de terceiro 
para funcionar corretamente. Se, por exemplo, o [unpkg](https://unpkg.com/#/) 
cair, e se você usar bibliotecas diretamente de lá, seu aplicativo cai junto.

### Code snippets online

Estes serviços são ideais para você experimentar bibliotecas front-end e 
dividir com as pessoas na internet. Você pode usar as bilbiotecas servidas 
pelos CDN's para testar as coisas mais rapidamente online.

- [jsbin](https://jsbin.com)
- [jsfiddle](https://jsfiddle.net/)
- [codepen](https://codepen.io/)

## Exercício

1. Escolha um dos serviços de snippets e transporte o index.html para lá.
2. Distribua seu snippet comentando o link para ele na issue de presença.
3. Descubra, no serviço escolhido, como criar um link para o snippet criado.

## Introdução ao Vue.js

- Biblioteca fron-end para aplicações sofisticadas
- Reatividade transparente
- Simples
- Flexível, escala "para cima" e "para baixo"
  - CDN, NPM e integração com *bundlers* e ferramentas *cli*
 

## Exercício
## Axios client-side
## Exercício axios client-side chamando server-side