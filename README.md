
const express = require('express');
const fs = require('fs').promises;
const app = express();
const port = 3000; // Você pode escolher outra porta

// Middleware para habilitar o CORS (permite requisições de diferentes domínios)
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Methods', 'GET');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
});

// Função para ler o arquivo JSON de forma assíncrona
async function lerEnderecos() {
  try {
    const data = await fs.readFile('enderecos.json', 'utf8');
    return JSON.parse(data);
  } catch (error) {
    console.error('Erro ao ler o arquivo de endereços:', error);
    return [];
  }
}

// Rota para buscar todos os endereços
app.get('/enderecos', async (req, res) => {
  const enderecos = await lerEnderecos();
  res.json(enderecos);
});

// Rota para buscar um endereço específico por ID (exemplo)
app.get('/enderecos/:id', async (req, res) => {
  const enderecos = await lerEnderecos();
  const enderecoId = parseInt(req.params.id);
  const endereco = enderecos.find(end => end.id === enderecoId);

  if (endereco) {
    res.json(endereco);
  } else {
    res.status(404).json({ mensagem: 'Endereço não encontrado' });
  }
});

app.listen(port, () => {
  console.log(`Servidor rodando em http://localhost:${port}`);
});
