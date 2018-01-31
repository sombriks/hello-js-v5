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


## Exercício
## Introdução ao Vue.js
## Exercício
## Axios client-side
## Exercício axios client-side chamando server-side