Configuração de um Cluster MongoDB com Docker

Este repositório contém as instruções para configurar um cluster MongoDB Replica Set utilizando Docker no Docker Desktop.

Requisitos

Antes de iniciar, certifique-se de ter instalado:

Docker Desktop (Baixar aqui)

MongoDB Compass (Baixar aqui)

Editor de texto (VS Code, Notepad++, etc.)

Passo 1: Criar uma Rede Docker

Execute o comando abaixo para criar uma rede chamada mongo-cluster para os contêineres MongoDB:

Passo 2: Criar os Contêineres do MongoDB

Criamos 4 nós MongoDB que serão usados no Replica Set.

Criando o Nó Primário

Criando os Nós Secundários

Passo 3: Inicializar o Replica Set

Acesse o terminal do mongo1 e execute o cliente MongoDB:

Dentro do console do MongoDB, execute:

Verifique o status do Replica Set:

Passo 4: Testar a Replicação

Abra o MongoDB Compass e conecte-se ao nó primário usando:

Inserindo Dados no Nó Primário

No terminal do mongo1, insira alguns dados:

Verifique se os dados foram replicados nos nós secundários.

Passo 5: Simular Queda de Nós

Simular Queda de um Nó Secundário

Verifique o status do cluster:

Para religar o nó:

Simular Queda do Nó Primário

Verifique no mongo2 ou mongo3 quem assumiu como primário:

Agora, insira um novo dado no novo primário:

Religue o nó original:

Passo 6: Gravar e Publicar o Vídeo

Grave a execução do cluster.

Publique no YouTube.

Na descrição do vídeo, coloque:

O link deste repositório.

O tempo de cada etapa do vídeo.

Exemplo:

Conclusão

Este guia ajudou a configurar um Replica Set do MongoDB no Docker Desktop, testando a replicação e a recuperação de falhas.

Se encontrar problemas, verifique os logs com:

Sinta-se à vontade para contribuir com melhorias!

Autor: [Seu Nome]Repositório: [Link do GitHub]
