# Teste do Cadê Clips no Windows

Este roteiro prepara uma primeira execução local do Cadê Clips no Windows. O objetivo do primeiro teste é confirmar que a aplicação abre, aceita um vídeo curto autorizado, processa o material e permite revisar/exportar os cortes. Não é necessário publicar nada.

## 1. O que será necessário

- Windows 10/11 de 64 bits com virtualização habilitada.
- Docker Desktop para Windows, usando o backend WSL 2.
- Git ou GitHub Desktop para baixar esta branch.
- Espaço livre em disco para imagens Docker, banco de dados, vídeo original e arquivos gerados. O primeiro build pode ocupar vários GB.
- Conexão com a internet durante o build e para baixar modelos/serviços externos.

O processamento local de vídeo e Whisper pode consumir bastante RAM, CPU e espaço. Comece com um arquivo curto (1–3 minutos), de preferência 720p. O desempenho depende do hardware; não há um requisito de RAM mínimo confiável definido pelo projeto neste momento.

## 2. Preparar o Windows

1. Instale o Docker Desktop: https://www.docker.com/products/docker-desktop/
2. Durante a configuração, mantenha **Use WSL 2 instead of Hyper-V** (quando oferecido).
3. Abra o Docker Desktop e espere aparecer que o mecanismo está em execução.
4. Se o Docker pedir para instalar/atualizar o WSL 2 ou reiniciar o Windows, conclua isso antes de continuar.
5. Baixe esta branch do repositório:
   - Abra https://github.com/meirelesinbound-create/cade-clips/tree/feature/cade-clips-direcao-produto
   - Use GitHub Desktop para clonar o repositório e selecionar a branch `feature/cade-clips-direcao-produto`, ou baixe o ZIP dessa branch.
   - Não faça alterações na branch `main`.

## 3. Criar a configuração local

Na pasta do projeto, abra o **Prompt de Comando (cmd)** e execute:

```bat
copy .env.example .env
```

Abra o arquivo `.env` no Bloco de Notas. Para um teste sem serviço de transcrição pago, ajuste estas linhas:

```env
TRANSCRIPTION_PROVIDER=whisper
WHISPER_MODEL_SIZE=tiny
BACKEND_CPU_ONLY=true
BACKEND_PLATFORM=linux/amd64
SELF_HOST=true
FRONTEND_PORT=3107
NEXT_PUBLIC_APP_URL=http://localhost:3107
BETTER_AUTH_URL=http://localhost:3107
```

O modelo Whisper `tiny` é propositalmente escolhido para o primeiro teste: reduz o download e o consumo em comparação com modelos maiores, mas pode errar mais, especialmente em português, áudio ruim ou falas sobrepostas. Depois que o fluxo funcionar, podemos testar `base` ou `small`.

### Escolher o provedor de IA para selecionar os cortes

A transcrição Whisper local não substitui o modelo de linguagem (LLM). O sistema ainda precisa de um LLM para analisar a transcrição e sugerir os trechos.

**Opção A — Ollama local:** se o Ollama já estiver instalado e um modelo compatível já estiver baixado, configure:

```env
LLM=ollama:NOME_DO_MODELO
OLLAMA_BASE_URL=http://host.docker.internal:11434/v1
```

Substitua `NOME_DO_MODELO` pelo identificador exato mostrado no Ollama. O modelo precisa conseguir seguir instruções e analisar texto; modelos muito pequenos podem iniciar, mas produzir sugestões fracas ou falhar. O serviço do Ollama precisa estar ativo no Windows. Não baixe um modelo grande antes de verificarmos a memória disponível.

**Opção B — provedor de IA por API:** se não houver um modelo local utilizável, escolha um provedor suportado e configure a variável `LLM` e sua chave correspondente no `.env` (por exemplo, `LLM=google-gla:gemini-3-flash-preview` e `GOOGLE_API_KEY=...`). Confira os limites e eventuais cobranças diretamente no provedor; não presuma que a API seja gratuita.

Não preencha nem compartilhe chaves aqui. O arquivo `.env` é local e não deve ser enviado ao GitHub.

## 4. Iniciar o Cadê Clips

Ainda no Prompt de Comando, dentro da pasta do projeto:

```bat
docker compose up -d --build
```

O primeiro build pode demorar bastante porque baixa imagens e instala dependências. Depois, confira o estado:

```bat
docker compose ps
```

Quando os serviços estiverem iniciados, abra:

- Aplicação: http://localhost:3107
- API: http://localhost:8000
- Documentação da API: http://localhost:8000/docs

Crie uma conta local na aplicação. O modo self-host fica ativado por padrão e não exige Stripe, e-mail transacional ou configuração de monetização para o teste básico.

## 5. Teste funcional controlado

1. Use um vídeo curto que você tenha autorização para processar. Para o primeiro teste, prefira um arquivo local em vez de importar de uma plataforma.
2. Faça upload do vídeo.
3. Selecione o idioma português, se a tela oferecer essa opção; caso contrário, deixe a detecção automática.
4. Inicie o processamento e aguarde a tarefa terminar.
5. Abra os cortes sugeridos e confira se os tempos fazem sentido, se as falas estão corretas e se cada trecho tem contexto suficiente.
6. Ajuste início/fim e legendas no editor.
7. Exporte um corte e confira o arquivo gerado: reprodução, áudio, sincronização, proporção vertical e resolução real.

Não vamos assumir que o preset vertical seja 1080×1920 até inspecionar o arquivo exportado. O primeiro teste também deve registrar se o enquadramento corta rostos ou informações importantes.

## 6. Se algo der errado

Na pasta do projeto, use:

```bat
docker compose ps
docker compose logs --tail=150 backend
docker compose logs --tail=150 worker
docker compose logs --tail=150 frontend
```

Para parar os serviços sem apagar os dados:

```bat
docker compose down
```

Para iniciar novamente:

```bat
docker compose up -d
```

**Não execute** `docker compose down -v` durante o teste: a opção `-v` apaga os volumes, incluindo os dados locais do banco e os arquivos persistidos.

Se o processamento falhar, envie o erro da tela e os trechos relevantes dos logs de backend e worker. Remova tokens, chaves e qualquer dado privado antes de compartilhar.

## 7. Limites importantes

- Whisper local evita a chave de transcrição AssemblyAI, mas usa CPU/RAM e pode ser lento.
- A seleção dos cortes ainda depende de um LLM local ou de uma API. Ollama local evita cobrança por chamada, mas exige hardware suficiente e um modelo capaz.
- B-roll do Pexels, integrações sociais, publicação e recursos de hospedagem não são necessários para validar o fluxo básico.
- Importar vídeos de plataformas pode depender de disponibilidade técnica, regras da plataforma e permissões. Não há suporte a contornar DRM ou restrições de acesso.
- Ter acesso a um vídeo não significa automaticamente ter autorização para republicá-lo. Use apenas material próprio, licenciado ou cujo uso esteja amparado por autorização ou exceção legal aplicável.
