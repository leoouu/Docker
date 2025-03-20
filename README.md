<!DOCTYPE html>
<html lang="pt-BR">
<body>
    <h1>🚀 Configuração de um Cluster MongoDB com Docker</h1>
    <p>Este repositório contém as instruções detalhadas para configurar um cluster <strong>MongoDB Replica Set</strong> utilizando <strong>Docker</strong> no <strong>Docker Desktop</strong>.</p>
    
    <h2>📌 Requisitos</h2>
    <ul>
        ✅ <a href="https://www.docker.com/products/docker-desktop">Docker Desktop</a></li>
        ✅ <a href="https://www.mongodb.com/try/download/compass">MongoDB Compass</a></li>
        ✅ Editor de texto (VS Code, Notepad++, etc.)</li>
    </ul>

    <h2>🔧 Passo 1: Criar uma Rede Docker</h2>
    <pre><code>docker network create mongo-cluster</code></pre>

    <h2>📦 Passo 2: Criar os Contêineres do MongoDB</h2>
    <h3>🌟 Criando o Nó Primário</h3>
    <pre><code>docker run -d --name mongo1 --net mongo-cluster -p 27017:27017 mongo:latest --replSet rs0</code></pre>
    <h3>🌟 Criando os Nós Secundários</h3>
    <pre><code>docker run -d --name mongo2 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo4 --net mongo-cluster mongo:latest --replSet rs0</code></pre>

    <h2>⚙️ Passo 3: Inicializar o Replica Set</h2>
    <pre><code>docker exec -it mongo1 mongosh</code></pre>
    <pre><code>rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo1:27017" },
    { _id: 1, host: "mongo2:27017" },
    { _id: 2, host: "mongo3:27017" },
    { _id: 3, host: "mongo4:27017" }
  ]
})</code></pre>
    <pre><code>rs.status()</code></pre>

    <h2>🛠️ Passo 4: Testar a Replicação</h2>
    <p>Abra o <strong>MongoDB Compass</strong> e conecte-se ao <strong>nó primário</strong> usando:</p>
    <pre><code>mongodb://localhost:27017/?replicaSet=rs0</code></pre>
    <h3>📥 Inserindo Dados no Nó Primário</h3>
    <pre><code>use testDB

db.testCollection.insertMany([
  { nome: "João", idade: 30 },
  { nome: "Maria", idade: 25 }
])</code></pre>

    <h2>⚠️ Passo 5: Simular Queda de Nós</h2>
    <h3>📉 Queda de um Nó Secundário</h3>
    <pre><code>docker stop mongo2</code></pre>
    <pre><code>rs.status()</code></pre>
    <pre><code>docker start mongo2</code></pre>
    
    <h3>📉 Queda do Nó Primário</h3>
    <pre><code>docker stop mongo1</code></pre>
    <pre><code>docker exec -it mongo2 mongosh
rs.status()</code></pre>
    <pre><code>use testDB
db.testCollection.insertOne({ nome: "Carlos", idade: 40 })</code></pre>
    <pre><code>docker start mongo1</code></pre>

    <h2>🎥 Passo 6: Gravar e Publicar o Vídeo</h2>
    <p>Grave a execução do cluster, publique no YouTube e inclua:</p>
    <ul>
        <li>📌 O link deste repositório.</li>
        <li>📌 O tempo de cada etapa do vídeo.</li>
    </ul>
    <pre><code>00:01:10 - Apresentação do grupo
00:10:15 - Explicação sobre Replica Set
00:14:30 - Inserção de dados no Primário</code></pre>

    <h2>✅ Conclusão</h2>
    <p>Este guia ajudou a configurar um <strong>Replica Set do MongoDB</strong> no <strong>Docker Desktop</strong>, testando a <strong>replicação</strong> e a <strong>recuperação de falhas</strong>.</p>
    <pre><code>docker logs mongo1</code></pre>
    <p>Sinta-se à vontade para contribuir com melhorias!</p>

    <h2>👨‍💻 Autor</h2>
    <p><strong>Nome:</strong> [Seu Nome] <br>
       <strong>Repositório:</strong> <a href="#">[Link do GitHub]</a></p>
</body>
</html>
