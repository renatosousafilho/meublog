---
layout: post
title: "Nodejs - Usando REST com express"
date: 2013-02-08 08:46
comments: true
categories: nodejs
---

<p>O atual contexto das aplicações webs é capaz de forncecer bem mais do que o processamento de páginas, em um contexto em que estamos cercados por redes sociais e web-services, pensar numa aplicação além de conteúdo HTML é nescessário. Gigantes como o Facebook e o Twitter são bastantes disseminados por permitir a extensão de suas funcionalidades para outras aplicações através de suas próprias APIs, permitindo que outros desenvolvedores utilizem seus recursos para desenvolver novas aplicações. Aí que entra o REST, um padrão de processamento de requisições que pode tranformar sua aplicação em uma eficaz API, de forma simples e bem transparente. O conceito de REST é um pouco abrangente, mas em uma definĩção curta e grossa é uma forma de associamos um recurso de uma aplicação através de um identificador único(caso queira saber mais sobre REST, leia <a href="http://www.infoq.com/br/articles/rest-introduction">este artigo</a>.)</p>

<h3>Criando nossa aplicãção</h3>

<p>O Express é um módulo do nodejs que nos permite escrever uma aplicação utilizando o padrão REST. Bastante similar ao Sinatra, framework para aplicações web do Ruby, ele consiste em especificar um comportamento para uma determinada URL de nossa aplicação. Mas antes devemos especificar as dependências de nossa aplicação da seguinte forma:</p>

<pre><code># package.json
{
  "name": "express-beginner",
  "description": "Apllication to show how to begin with express",
  "version": "0.0.1",
  "dependencies": {
     "express": "3.x"
  }
}
</code></pre>
<p>Agora vamos criar o código de nossa aplicação. Por ora, iremos fazer o mapeamento da url <u>http://localhost:3000/</u> para exibir a string "Hello World".
</p>

<pre><code># server.js
var express = require("express");
var app = express();

app.get("/", function(req, res){
   res.send("Hello World! \n")
});

app.listen(3000);
console.log("Application is running in http://localhost:3000");
</code></pre>

<p>Podemos acessar a rota criada fazendo um requisicão para a URL http://localhost:3000 através do browser ou através do cURL.</p>

<pre><code>curl "http://localhost:3000"</code></pre>

<p>O padrão REST possui 4 principais classificadores para tratar requisições, no nosso exemplo anterior só utilizamos o GET que é o classificador de uma requisição padrão. Além do GET, poderemos utilizar os classificadores POST, PUT e DELETE usando os métodos correspodentes na API do Express.</p>

<p>Para ilustrar melhor vamos construir uma pequena API com as operações básicas de um CRUD para uma lista de contatos</p>

<pre><code>#server.js
...
contacts = []

app.use(express.bodyParser());

var posts = []

app.get('/contacts',function(req,res){
  res.send(contacts)
});

app.post('/contacts', function(req,res){
  var id = contacts.length + 1;
  var name = req.body.title;
  var contact = {id:id, name:name}
  contacts.push(contact);
  res.send(contacts);
});

app.put('/contacts/:id', function(req,res){
  var id = req.params.id;
  contacts[id-1].title = req.body.name;
  res.send(contacts[id-1]);
});

app.delete('/contacts/:id', function(req,res){
  var id = req.params.id;
  contact = contacts.splice(id-1,1)
  res.send("The post was removed");
});

app.listen(3000);
console.log("Application is running in http://localhost:3000");

</code></pre>

<p>Podemos notar que não há muita dificuldade no uso da api, como já vimos temos o get que processa requisições do tipo GET e consequentemente o post, put e delete que correspondem as operações REST de mesmo nome. O que podemos observar são alguns detalhes. Primeiro, á chamada de app.use(express.bodyParser()) que serve para ajustar o body, ou seja, o corpo da requisiçao para uma forma que somos capazes de manipular via JSON, note que dentro da função anônima associada a app.post, obtemos os valores da requisição através de req.body, isto é o corpo da nossa requisição, onde se encontra dentre outras informações os atributos que definirmos na nossa chamada deste caminho. No caso enviamos o atributo name que será preecnhido com o nome do novo contato que vai ser salvo sempre que fizermos uma requisição POST para o caminho '/contacts'.  Outro detalhe é a passagem de parâmetros enviados pela URL, note que para <i>put</i> e <i>delete</i> definimos a URL com o sufixo :id, isso serve para dizer que quando for feita uma requisiçao REST para um desses métodos, a requisição receberá  um parâmetro chamado a id que pode ser recuperado através do objeto <i>req.params</i>.</p>
<p>Para testarmos a aplicação basta iniciarmos ela com o comando <b>node</b> e fazer as requisições usando o cURL.</b>

<pre><code>
node server.js
curl -X GET http://localhost/contacts	
curl -X -d "name=Jovem Nerd" ṔOST http://localhost/contacts	
curl -X -d "name=Azaghal" ṔOST http://localhost/contacts	
curl -X -d "name=Gaveta" ṔOST http://localhost/contacts	
curl -X GET http://localhost/contacts	
curl -X PUT -d "name=Alexandre Ottoni" http://localhost/contacts/1
[{id:1, name:"Jovem Nerd"}, {id:2, name:"Azaghal"},{id:3, name:"Gaveta"}]
curl -X GET http://localhost/contacts	
[{id:1, name:"Alexandre Ottoni"}, {id:2, name:"Azaghal"},{id:3, name:"Gaveta"}]
curl -X DELETE http://localhost/contacts/3
curl -X GET http://localhost/contacts	
[{id:1, name:"Alexandre Ottoni"}, {id:2, name:"Azaghal"}]
</code></pre>

<p>É isso aí pessoal, acima vemos a demonstração de uma singela aplicação REST que controla uma lista de contatos. Obviamente que se estívessesmos trablahando com alguma banco de dados o código seria um pouco diferente, porém o exemplo está aí para ilustrar o uso desse módulo do NodeJS.</p>