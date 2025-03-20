docker run -d --name mongo3 --net mongo-cluster mongo:latest --replSet rs0

docker run -d --name mongo4 --net mongo-cluster mongo:latest --replSet rs0

_id: "rs0",
members: [
{ _id: 0, host: "mongo1:27017" },
{ _id: 1, host: "mongo2:27017" },
{ _id: 2, host: "mongo3:27017" },
{ _id: 3, host: "mongo4:27017" }
]
})
rs.status()

db.testCollection.insertMany([
{ nome: "João", idade: 30 },
{ nome: "Maria", idade: 25 }
])

rs.status()
use testDB
db.testCollection.insertOne({ nome: "Carlos", idade: 40 })
docker start mongo1

00:10:15 - Explicação sobre Replica Set
00:14:30 - Inserção de dados no Primário
