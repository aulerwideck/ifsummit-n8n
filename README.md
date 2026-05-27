# Automatizando processos com N8N

Materiais do minicurso **Automatizando processos com N8N**, apresentado no **IF SUMMIT 2026**.

O repositório contém slides, apostila, roteiro do instrutor, exercícios, workflow importável e ambiente local com Docker Compose para executar o n8n durante a prática.

## Informações do Minicurso

- **Evento:** IF SUMMIT 2026
- **Data:** 27/05/2026
- **Ministrante:** Thiago Auler Wideck
- **Público:** estudantes de ADS, comunidade acadêmica e participantes com pouco ou nenhum contato prévio com n8n

## Objetivo

Construir, passo a passo, um fluxo de automação no n8n para receber dados de cadastro, validar CNPJ, consultar uma API pública e retornar uma resposta estruturada.

Durante o minicurso, os participantes praticam:

- conceitos de workflow, node, trigger, action e execution;
- tráfego de dados em JSON;
- uso de Webhook;
- validação e transformação de dados;
- consumo de API REST com HTTP Request;
- consulta de CNPJ na BrasilAPI;
- condicionais;
- depuração por execuções;
- extensão do fluxo com IA usando Gemini.

## Conteúdo Principal

| Arquivo | Descrição |
| --- | --- |
| [apostila-do-aluno.md](apostila-do-aluno.md) | Guia de apoio para os participantes. |
| [exercicios-praticos.md](exercicios-praticos.md) | Atividades práticas e desafios extras. |
| [workflow-triagem-simples.json](workflow-triagem-simples.json) | Workflow simples importável no n8n. |
| [formulario-cadastro.html](formulario-cadastro.html) | Formulário HTML para enviar dados ao webhook. |
| [N8N_IF_SUMMIT_2026.pdf](N8N_IF_SUMMIT_2026.pdf) | Slides em PDF. |

## Rodando o n8n Localmente

### Requisitos

- Docker Desktop ou Docker Engine com Docker Compose.
- Porta `5678` livre.
- Navegador atualizado.

### Subir o Ambiente

```bash
cp .env.example .env
mkdir -p local-files
docker compose up -d
```

Depois acesse:

```text
http://localhost:5678
```

No primeiro acesso, crie o usuário dono da instância local.

### Comandos Úteis

```bash
docker compose ps
docker compose logs -f n8n
docker compose stop
docker compose down
```

Para apagar também os dados salvos:

```bash
docker compose down -v
```

## Estrutura do Ambiente

O arquivo [compose.yaml](compose.yaml) sobe um container do n8n com:

- imagem `docker.n8n.io/n8nio/n8n:stable`;
- porta local `5678`;
- timezone `America/Sao_Paulo`;
- volume Docker `n8n_data` em `/home/node/.n8n`;
- pasta local `local-files` montada como `/files` dentro do container.

As variáveis iniciais estão em [.env.example](.env.example). Para usar uma URL pública de webhook, atualize `WEBHOOK_URL` no arquivo `.env`.

## Workflow do Minicurso

O fluxo principal segue esta sequência:

1. Formulário HTML envia dados via `POST`.
2. Webhook do n8n recebe o payload.
3. Node de código normaliza e valida o CNPJ.
4. HTTP Request consulta a BrasilAPI.
5. Condicional separa CNPJ válido e inválido.
6. Resposta final retorna dados estruturados.

Para importar o fluxo:

1. Abra `http://localhost:5678`.
2. Acesse a tela de workflows.
3. Use a opção de importar workflow.
4. Selecione [workflow-triagem-simples.json](workflow-triagem-simples.json).
5. Execute o workflow em modo de teste.
6. Envie dados pelo [formulario-cadastro.html](formulario-cadastro.html).

## Referências

- Documentação oficial do n8n: <https://docs.n8n.io/>
- Instalação com Docker: <https://docs.n8n.io/hosting/installation/docker/>
- Docker Compose para n8n: <https://docs.n8n.io/hosting/installation/server-setups/docker-compose/>
- BrasilAPI CNPJ: <https://brasilapi.com.br/docs#tag/CNPJ>