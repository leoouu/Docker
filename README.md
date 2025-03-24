<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>MongoDB Replica Set com Docker</title>
    <style>
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            line-height: 1.6;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f8f9fa;
            color: #333;
        }

        h1, h2 {
            color: #2c3e50;
            border-bottom: 2px solid #3498db;
            padding-bottom: 5px;
        }

        code {
            background-color: #f4f4f4;
            padding: 2px 5px;
            border-radius: 3px;
            font-family: 'Courier New', monospace;
        }

        pre {
            background-color: #282c34;
            color: #abb2bf;
            padding: 15px;
            border-radius: 5px;
            overflow-x: auto;
            margin: 20px 0;
        }

        .container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .emoji {
            font-size: 1.2em;
            margin-right: 8px;
        }

        ul {
            padding-left: 25px;
        }

        li {
            margin-bottom: 8px;
        }

        .note {
            background-color: #e3f2fd;
            padding: 15px;
            border-left: 4px solid #2196F3;
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🛠 Configuração de MongoDB Replica Set com Docker</h1>

        <h2>📋 Pré-requisitos</h2>
        <ul>
            <li><span class="emoji">✅</span> Docker Desktop instalado</li>
            <li><span class="emoji">🗄️</span> MongoDB Compass (opcional para visualização)</li>
            <li><span class="emoji">⌨️</span> Editor de texto (VS Code recomendado)</li>
        </ul>

        <div class="note">
            <strong>💡 Dica:</strong> Verifique as portas disponíveis antes de iniciar!
        </div>

        <h2>🌐 Criando Rede Docker</h2>
        <pre><code>docker network create mongo-cluster</code></pre>

        <h2>🐳 Configurando Contêineres</h2>
        <pre><code># Nó Primário (mongo1)
docker run -d --name mongo1 --net mongo-cluster -p 27017:27017 mongo:latest --replSet rs0

# Nós Secundários
docker run -d --name mongo2 --net mongo-cluster mongo:latest --replSet rs0
docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0</code></pre>

        <h2>⚙️ Inicializando o Replica Set</h2>
        <pre><code>docker exec -it mongo1 mongosh --eval "rs.initiate({
  _id: 'rs0',
  members: [
    { _id: 0, host: 'mongo1:27017' },
    { _id: 1, host: 'mongo2:27017' },
    { _id: 2, host: 'mongo3:27017' }
  ]
})"</code></pre>

        <h2>🧪 Testando a Replicação</h2>
        <pre><code>use testDB
db.testCollection.insertMany([
  { nome: "João", idade: 30 },
  { nome: "Maria", idade: 25 }
])</code></pre>

        <h2>⚠️ Simulando Falha</h2>
        <pre><code># Parar o nó primário
docker stop mongo1

# Verificar status no nó secundário
docker exec -it mongo2 mongosh --eval "rs.status()"

# Reiniciar nó
docker start mongo1</code></pre>

        <h2>✅ Conclusão</h2>
        <p>Seu ambiente MongoDB Replica Set está pronto com:</p>
        <ul>
            <li>📊 3 nós configurados</li>
            <li>🔄 Replicação automática</li>
            <li>⏱️ Failover automático</li>
        </ul>

        <div class="note">
            <strong>🔌 Conexão MongoDB Compass:</strong><br>
            URI: <code>mongodb://localhost:27017/?replicaSet=rs0</code>
        </div>
    </div>
</body>
</html>
