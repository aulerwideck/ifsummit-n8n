# Apostila do Aluno

## O Que Vamos Construir

Um fluxo no n8n para triagem de inscrições/solicitações de evento, com:

- captura de dados;
- consulta de BrasilAPI por CNPJ;
- decisão condicional;
- classificação por IA no fluxo avançado.

## O Que É o n8n

O n8n é uma ferramenta de automação visual para conectar sistemas e executar fluxos de trabalho. Ele é útil quando um processo precisa:

- receber dados;
- transformar dados;
- consultar outro serviço;
- tomar uma decisão;
- disparar uma ação.

## Conceitos Básicos

- Workflow: sequência de passos.
- Node: cada bloco executado.
- Trigger: o que inicia o fluxo.
- Action: o que processa ou envia dados.
- JSON: formato de dados usado entre os nodes.

## Rodando o n8n Localmente

O ambiente do minicurso usa Docker Compose para evitar instalações diferentes em cada máquina.

### Requisitos

- Docker Desktop ou Docker Engine instalado.
- Porta `5678` livre.
- Arquivos do minicurso baixados do repositório de apoio: `https://github.com/aulerwideck/`.

### Subir o Ambiente

No terminal, dentro da pasta do projeto:

```bash
cp .env.example .env
mkdir -p local-files
docker compose up -d
```

Depois acesse:

```text
http://localhost:5678
```

Na primeira abertura, o n8n pedirá a criação do usuário dono da instância local.

### Comandos Úteis

```bash
docker compose ps
docker compose logs -f n8n
docker compose stop
docker compose down
```

### Onde Ficam os Dados

- Workflows, credenciais e configurações ficam no volume Docker `n8n_data`.
- Arquivos compartilhados com nodes de leitura/escrita ficam em `local-files` no computador e em `/files` dentro do n8n.
- Para limpar completamente o ambiente, use `docker compose down -v`. Isso apaga os dados persistidos.

## Fluxo Simples

### Passo 1

Receber os dados de cadastro por formulário ou webhook, com CNPJ obrigatório.

### Passo 2

Organizar os campos e conferir o que chegou.

### Passo 3

Consultar a BrasilAPI usando o CNPJ informado.

### Passo 4

Gerar uma resposta simples para o usuário ou para a equipe.

### Passo 5

Usar uma condição para separar casos válidos e inválidos.

## Fluxo Avançado

No fluxo avançado, os dados seguem para o Gemini para classificação. A ideia é identificar, por exemplo:

- prioridade;
- categoria;
- interesse;
- urgência;
- nível de atendimento;
- tipo de participação.

## Expressões Mais Usadas

```js
{{ $json.cnpj }}
{{ $json.nome }}
{{ $json.email }}
{{ $json.cidade }}
{{ $json.tipo_participacao }}
```

## O Que Verificar Em Cada Teste

- Se o node recebeu dados.
- Se o JSON está com os campos esperados.
- Se a consulta de CNPJ retornou dados coerentes.
- Se a condição enviou para o caminho certo.
- Se a classificação da IA está coerente.

## Webhook Público

Como o n8n estará local, o webhook precisa ser publicado na internet para receber o cadastro do site.

Opção recomendada:

- Cloudflare Tunnel.

Alternativa:

- ngrok.

## Dicas De Estudo

- Entender JSON ajuda muito.
- Entender HTTP, GET e POST ajuda a depurar melhor.
- Node a node é mais fácil do que tentar montar tudo de uma vez.
- Erro em automação não é fim de trabalho, é dado para depurar.

## Exercícios Extras

1. Trocar os campos do formulário.
2. Adicionar um campo de telefone.
3. Mudar a API pública usada.
4. Criar uma saída diferente para cada categoria.
5. Ajustar o prompt do Gemini.
