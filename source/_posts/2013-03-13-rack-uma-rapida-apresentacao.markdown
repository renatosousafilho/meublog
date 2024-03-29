---
layout: post
title: "Rack - Uma rápida apresentação"
date: 2013-03-13 14:26
comments: true
categories: Rack, Ruby
---

<p>Antes de começar a escrever qualquer coisa sobre Ruby on Rails ou Sinatra, decidi fazer um rápido estudo de algo que, particulamente, nunca dei a devida atenção até então, a Gem que permite o desenvolvimento do lado do Servidor com ruby, o Rack.</p>
<p>Similar ao último post sobre nodejs, não faremos nada de tão avançado, vamos apenas nos apresentarmos a este recurso do Ruby que é de suma importância para o funcionamento dos seus já citados e famosos frameworks. O Rack serve como uma camada intermediária entre um código ruby e um servidor web através de sua API. O primeiro passo para sua utilização, com o ruby devidamente instalado é instalar a gem.</p>
<pre><code>gem install rack</code></pre>
<p>Seguindo o padrão de qualquer servidor, o código que irá criar nosso servidor deve implementar um tratamento para uma requisição para processar e devolver uma resposta para o cliente. A estrutura básica de qualquer servidor implementado com Rack consiste em criar uma classe com um método 'call' que receba uma hash com as informações da requisição e que retorne uma outra hash com uma resposta para o usuário, que deve incluir informaçoes do cabeçalho e do corpo.</p>
<pre><code># hello_world.rb
class HelloWorld
   def call(env)
     [200,{"Content-Type":"200"},["Hello World!"]]
    end
end
</code></pre>
<p>Agora basta escrevermos o arquivo config.ru que será usado para subir nossa aplicação.</p>
<pre><code># config.ru
require 'rack'
require './hello_world.rb'

run HelloWorld.new
</code></pre>

<p>Para subirmos a aplicação basta utilizar o comando:</p>
<pre><code>rackup config.ru</code></pre>

<p>A aplicação vai ser iniciada por padrão na porta 9292 e ao acessarmos  http://localhost:9292, teremos nossa aplicação retornando a mensagem espeficicada. Embora o Ruby on Rails e o Sinatra já implemetem o Rack por baixo dos panos é importante conhecermos um pouco a respeito, pois usando o Rack podemos criar nossos pŕoprios frameworks escritos em Ruby ou mesmo para fazermos algum tipo de alteração no core dos frameworks já existentes.
</p>


