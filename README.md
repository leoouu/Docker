<h2>📌 Requisitos</h2>
<ul>
    <li>✅ Docker Desktop</li>
    <li>✅ MongoDB Compass</li>
    <li>✅ Editor de texto (VS Code, Notepad++)</li>
</ul>

<h2>🔧 Criando a Rede Docker</h2>
<pre><code>docker network create mongo-cluster</code></pre>

<h2>📦 Criando os Contêineres</h2>
<pre><code>docker run -d --name mongo1 --net mongo-cluster -p 27017:27017 mongo:latest --replSet rs0
docker run -d --name mongo2 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0

<h2>⚙️ Inicializando o Replica Set</h2>
<pre><code>docker exec -it mongo1 mongosh</code></pre>
<pre><code>rs.initiate({

_id: "rs0",
members: [
{ _id: 0, host: "mongo1:27017" },
{ _id: 1, host: "mongo2:27017" },
{ _id: 2, host: "mongo3:27017" }
]
})
rs.status()

<h2>📥 Testando a Replicação</h2>
<pre><code>use testDB 
db.testCollection.insertMany([
{ nome: "João", idade: 30 },
{ nome: "Maria", idade: 25 }
])

<h2>📉 Simulando Queda de Nó</h2>
<pre><code>docker stop mongo1</code></pre>
<pre><code>docker exec -it mongo2 mongosh

rs.status()
docker start mongo1

<h2>✅ Conclusão</h2>
<p>Agora seu <strong>Replica Set do MongoDB</strong> está configurado e funcional no <strong>Docker Desktop</strong>!</p>
