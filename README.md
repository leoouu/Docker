<h1>🛠 Configuração MongoDB Replica Set com Docker</h1>

<h2>📋 Pré-requisitos</h2>
<ul>
    <li>✅ <a href="https://www.docker.com/products/docker-desktop/" target="_blank" rel="noopener noreferrer">Docker Desktop</a> instalado</li>
    <li>🗄️ <a href="https://www.mongodb.com/try/download/compass" target="_blank" rel="noopener noreferrer">MongoDB Compass</a> (opcional)</li>
    <li>⌨️ Editor de texto (VS Code recomendado)</li>
</ul>

<h2>🌐 Criar Rede Docker</h2>
<pre><code>docker network create mongo-cluster</code></pre>

<h2>🐳 Configurar Contêineres</h2>
<pre><code><h2># Nó Primário</h2>
docker run -d --name mongo1 --net mongo-cluster -p 27017:27017 mongo:latest --replSet rs0

# Nós Secundários
docker run -d --name mongo2 --net mongo-cluster mongo:latest --replSet rs0
docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0</code></pre>

<h2>⚙️ Inicializar Replica Set</h2>
<pre><code>docker exec -it mongo1 mongosh --eval "rs.initiate({
  _id: 'rs0',
  members: [
    { _id: 0, host: 'mongo1:27017' },
    { _id: 1, host: 'mongo2:27017' },
    { _id: 2, host: 'mongo3:27017' }
  ]
})"</code></pre>

<h2>🧪 Testar Replicação</h2>
<pre><code>use testDB
db.testCollection.insertMany([
  { nome: "João", idade: 30 },
  { nome: "Maria", idade: 25 }
])</code></pre>

<h2>⚠️ Simular Falha</h2>
<pre><code># Parar nó primário
docker stop mongo1

# Verificar status
docker exec -it mongo2 mongosh --eval "rs.status()"

# Reiniciar nó
docker start mongo1</code></pre>

<h2>✅ Funcionalidades</h2>
<ul>
    <li>📊 3 nós MongoDB em cluster</li>
    <li>🔄 Replicação automática de dados</li>
    <li>⏱️ Failover automático</li>
</ul>

<blockquote>
    <strong>🔌 Conexão MongoDB Compass:</strong><br>
    URI: <code>mongodb://localhost:27017/?replicaSet=rs0</code>
</blockquote>
