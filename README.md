<h1>🛠 Configuração MongoDB Replica Set com Docker</h1>

<h2>📋 Pré-requisitos</h2>
<ul>
    <li>✅ <a href="https://www.docker.com/products/docker-desktop/" target="_blank" rel="noopener noreferrer">Docker Desktop</a> instalado</li>
    <li>🗄️ <a href="https://www.mongodb.com/try/download/compass" target="_blank" rel="noopener noreferrer">MongoDB Compass</a> (opcional)</li>
    <li>⌨️ Editor de texto (VS Code recomendado)</li>
</ul>

<h2>📱 Baixar a imagem do MongoDB</h2>
<pre><code>docker pull mongodb/mongodb-community-server:latest</code></pre>

<h2>🌐 Criar Rede Docker</h2>
<pre><code>docker network create mongo-cluster</code></pre>

<h2>🐳 Configurar Contêineres</h2>
<pre><code><h2>Nó Primário</h2>
docker run -d --rm -p 27017:27017 --name mongo1 --network mongo-cluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,mongo1

# Nós Secundários
docker run -d --rm -p 27018:27017 --name mongo2 --network mongo-cluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,mongo2
docker run -d --rm -p 27019:27017 --name mongo3 --network mongo-cluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,mongo3
docker run -d --rm -p 27020:27017 --name mongo4 --network mongo-cluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,mongo4</code></pre>

<h2>⚙️ Inicializar Replica Set</h2>
<pre><code>docker exec -it mongo1 mongosh

rs.initiate ({ _id: "myReplicaSet", members:[{_id:0, host: "mongo1"}, {_id:1, host: "mongo2"}, {_id:2, host: "mongo3"}, {_id:3, host: "mongo4"}, {_id:4, host: "mongo5"}, {_id:5, host: "mongo6"}]});"</code></pre>

<h2>🧪 Testar Replicação</h2>
<pre><code>use testdb;

db.test.insertMany([
    {"nome": "Ana", "idade": 20},
    {"nome": "Bruno", "idade": 21},
    {"nome": "Caio", "idade": 22},
    {"nome": "Diana", "idade": 23}
]);
db.test.find();</code></pre>

<h2>🔌 Conectar-se ao cluster</h2>
<pre><code>mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.8</code></pre>

<h2>⚠️ Simular Falha</h2>
<pre><code><h2> Parar nó primário </h2>
docker stop mongo1

# Verificar status
docker exec -it mongo2 mongosh --eval "rs.status()"

# Reiniciar nó
docker start mongo1</code></pre>

<h2>✅ Funcionalidades</h2>
<ul>
    <li>📊 4 nós MongoDB em cluster</li>
    <li>🔄 Replicação automática de dados</li>
    <li>⏱️ Failover automático</li>
</ul>

<blockquote>
    <strong>🔌 Conexão MongoDB Compass:</strong><br>
    URI: <code>mongodb://localhost:27017/?replicaSet=rs0</code>
</blockquote>

<h2>👨‍💻 Autor</h2>
    <p><strong>Nome:</strong> Leonardo <br>
       <strong>Repositório:</strong> <a href="#">https://github.com/leoouu/Docker</a></p>
