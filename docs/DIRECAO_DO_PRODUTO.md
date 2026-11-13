# Direção do produto — Cadê Clips

## Objetivo

Transformar a base open source do SupoClip em uma ferramenta editorial do Cadê o Roteirista para preparar vídeos curtos sobre cinema e séries. O foco não é prometer viralização automática: é encontrar trechos relevantes, facilitar a revisão humana e entregar arquivos verticais consistentes para Reels.

## Fundação técnica

- Repositório-base: [FujiwaraChoki/supoclip](https://github.com/FujiwaraChoki/supoclip).
- Preservar avisos de copyright, atribuição e os termos da licença AGPL-3.0.
- Reaproveitar a infraestrutura existente de upload, transcrição, planejamento de cortes, fila de processamento, editor e renderização.
- Não reimplementar o pipeline de vídeo antes de identificar uma lacuna comprovada.

## Público e fluxo principal

1. A pessoa importa um arquivo de vídeo que tem direito de usar ou uma fonte suportada cujo uso seja permitido.
2. Escolhe o objetivo editorial: melhores momentos, trailer/teaser, assunto específico, resumo ou sequência em partes.
3. Informa o contexto (obra, episódio, personagens, tema, tom e eventuais restrições de spoiler).
4. O sistema analisa o material e apresenta cortes sugeridos com justificativas e marcações de tempo.
5. A pessoa revisa, ajusta início/fim, enquadramento e legendas.
6. Exporta os cortes em formato vertical e confere o arquivo final antes de publicar.

## Princípios editoriais

- Priorizar contexto, clareza e valor editorial, não apenas frases de efeito ou pontuação de viralidade.
- Respeitar spoilers: perguntar se o conteúdo pode revelar acontecimentos importantes e oferecer um modo sem spoilers.
- Não inventar falas, cenas, personagens ou contexto que não estejam no material.
- Não tratar reconhecimento facial como identificação de atores/personagens. A análise visual deve descrever elementos visuais sem presumir identidades.
- Sempre exigir revisão humana antes da publicação.
- A ferramenta não concede direitos de uso sobre trailers, episódios, músicas ou outros materiais protegidos. Não implementar bypass de DRM nem fluxos destinados a extrair conteúdo de serviços de streaming.

## Requisitos do primeiro MVP

### P0 — validar antes de personalizar
- Instalação local no Windows via Docker Desktop, documentando RAM, espaço em disco e dependências.
- Subir frontend, API, worker, Redis e PostgreSQL de acordo com a arquitetura upstream.
- Confirmar o fluxo completo de upload → análise → cortes → edição → exportação.
- Medir proporção, resolução, codec, áudio e sincronização do arquivo exportado. Não presumir que todo preset resulta em 1080×1920: validar no código e com um vídeo de teste.
- Verificar quais provedores de transcrição/LLM são realmente gratuitos e quais exigem chave, cota ou cobrança.

### P1 — identidade Cadê
- Nome, textos, cores e elementos visuais Cadê Clips.
- Presets de legenda legíveis e discretos, sem cobrir rostos ou informações importantes.
- Exportação vertical 9:16 com três estratégias selecionáveis: corte central/face-aware, enquadramento manual e preenchimento com fundo desfocado quando o corte prejudicar a composição.
- Prévia do enquadramento e área segura do Instagram antes de renderizar.

### P2 — especialização em cinema e séries
- Instruções editoriais em português.
- Campo de contexto da obra e do episódio, assunto/personagens e tolerância a spoilers.
- Ranking que combine relevância narrativa, qualidade audiovisual e independência do trecho.
- Análise visual de cenas e composição como etapa separada, adicionada somente após benchmark do pipeline textual existente.
- Detecção de mudanças de cena e suporte a trechos sem diálogo, sem presumir que o pipeline de transcrição resolva esses casos.

## Critérios de aceite do MVP

- Uma instalação limpa documentada no Windows consegue iniciar todos os serviços.
- Um vídeo de teste autorizado percorre o pipeline sem intervenção manual no backend.
- O usuário consegue revisar e ajustar cada corte.
- O MP4 final é inspecionado tecnicamente e corresponde ao preset escolhido (incluindo 9:16 e resolução declarada).
- O produto informa claramente quando um provedor depende de chave ou possui limites gratuitos.
- Nenhuma alteração é feita diretamente na branch `main`; mudanças seguem branches e PRs.

## Próxima sequência de implementação

1. Auditoria de setup e requisitos mínimos do upstream no Windows.
2. Execução de ponta a ponta com um vídeo de teste curto e autorizado.
3. Auditoria do render/export para confirmar 9:16 e 1080×1920 (ou identificar a alteração necessária).
4. Aplicar identidade visual Cadê em uma branch isolada.
5. Implementar o primeiro ajuste editorial especializado e cobrir com testes.
