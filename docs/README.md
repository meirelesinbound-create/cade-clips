# Documentação do Cadê Clips

Este diretório reúne a documentação da base SupoClip adaptada para o Cadê Clips. A implementação ainda está em fase de validação; não considere recursos ou formatos como confirmados até testá-los no fluxo real.

## Comece por aqui

1. [Teste no Windows](./TESTE_WINDOWS.md) — roteiro para instalar, iniciar e validar o fluxo no Windows.
2. [Direção do produto](./DIRECAO_DO_PRODUTO.md) — objetivo, escopo do MVP e princípios editoriais.
3. [Setup original](./setup.md) — arquitetura Docker, serviços e configuração geral.
4. [Configuração](./configuration.md) — provedores, variáveis de ambiente e modos de processamento.
5. [Guia do aplicativo](./app-guide.md) — telas e fluxos de uso.
6. [Arquitetura](./architecture.md) — frontend, backend, worker, fila e banco.
7. [Solução de problemas](./troubleshooting.md) — diagnóstico de falhas.

## Mapa da documentação

- [Teste no Windows](./TESTE_WINDOWS.md)
  - Docker Desktop com WSL 2
  - Configuração de transcrição Whisper local
  - Seleção de LLM local ou por API
  - Primeiro teste com vídeo curto
  - Diagnóstico e coleta de logs
- [Direção do produto](./DIRECAO_DO_PRODUTO.md)
  - Objetivo editorial
  - Escopo do MVP
  - Critérios de aceite
- [Setup](./setup.md)
  - Instalação Docker-first
  - Desenvolvimento local
  - Checklist de primeira execução
- [Configuração](./configuration.md)
  - Chaves e provedores de IA
  - Modos de processamento
  - Configurações de autenticação e self-host
- [Guia do aplicativo](./app-guide.md)
  - Telas e fluxos principais
  - Recursos administrativos
- [Arquitetura](./architecture.md)
  - Frontend, backend, worker, Redis e PostgreSQL
  - Pipeline de processamento de vídeo
- [API Reference](./api-reference.md)
  - Rotas frontend e endpoints backend
- [Desenvolvimento](./development.md)
  - Estrutura do repositório e comandos de teste
- [Troubleshooting](./troubleshooting.md)
  - Inicialização, tarefas, autenticação e desempenho

## Sobre a base técnica

O Cadê Clips reaproveita a base open source SupoClip. A aplicação combina frontend Next.js, API FastAPI, worker ARQ, Redis e PostgreSQL. O processamento de vídeo utiliza FFmpeg e serviços/modelos de transcrição e linguagem configuráveis.

A licença da base é AGPL-3.0. Consulte o arquivo [LICENSE](../LICENSE) e preserve os avisos e obrigações aplicáveis às modificações e à distribuição.
