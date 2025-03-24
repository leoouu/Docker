<h1>🚀 Configuração de um Cluster MongoDB com Docker</h1>

Este repositório contém as instruções detalhadas para configurar um cluster MongoDB Replica Set utilizando Docker no Docker Desktop.

📌 Requisitos

✅ <a href="https://www.docker.com/products/docker-desktop">Docker Desktop</a></li><br />
✅ <a href="https://www.mongodb.com/try/download/compass">MongoDB Compass</a></li><br />
✅ Editor de texto (VS Code, Notepad++, etc.)</li>

<h2>🔧 Passo 1: Criar uma Rede Docker </h2>
<pre><code>docker network create mongo-cluster</code></pre>


<h2>📦 Passo 2: Criar os Contêineres do MongoDB</h2>

🌟 Criando o Nó Primário
<pre><code>docker run -d --name mongo1 --net mongo-cluster -p 27017:27017 mongo:latest --replSet rs0</code></pre>

🌟 Criando os Nós Secundários
<pre><code>docker run -d --name mongo2 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo4 --net mongo-cluster mongo:latest --replSet rs0</code></pre>

<h2>⚙️ Passo 3: Inicializar o Replica Set </h2>
>docker exec -it mongo1 mongosh
    <<code>rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo1:27017" },
    { _id: 1, host: "mongo2:27017" },
    { _id: 2, host: "mongo3:27017" },
    { _id: 3, host: "mongo4:27017" }
  ]
})</code>
    <code>rs.status()</code>


<h2>🛠️ Passo 4: Testar a Replicação </h2>

Abra o MongoDB Compass e conecte-se ao nó primário usando:
<pre><code>mongodb://localhost:27017/?replicaSet=rs0</code></pre>

📥 Inserindo Dados no Nó Primário
<pre><code>use testDB

db.testCollection.insertMany([
  { nome: "João", idade: 30 },
  { nome: "Maria", idade: 25 }
])</code></pre>

<h2>⚠️ Passo 5: Simular Queda de Nós</h2>

📉 Queda de um Nó Secundário
<pre><code>docker stop mongo2</code></pre>
<pre><code>rs.status()</code></pre>
<pre><code>docker start mongo2</code></pre>

📉 Queda do Nó Primário
<pre><code>docker stop mongo1</code></pre>
    <pre><code>docker exec -it mongo2 mongosh
<pre>rs.status()</code></pre>
    <pre><code>use testDB
<pre>db.testCollection.insertOne({ nome: "Carlos", idade: 40 })</code></pre>
    <pre><code>docker start mongo1</code></pre>





<h1>✅ Conclusão</h1>

Este guia ajudou a configurar um Replica Set do MongoDB no Docker Desktop, testando a replicação e a recuperação de falhas.



<h3> 👨‍💻 Autor </h3>

Nome: Leonardo <br />
Repositório: [[Link do GitHub]](https://github.com/leoouu/Docker)
