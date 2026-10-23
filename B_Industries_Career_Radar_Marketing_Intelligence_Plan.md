# B. Industries — Especificação de execução
## Career Radar + Marketing Intelligence (MVP local)

**Status:** plano de implementação; nenhuma alteração local é declarada como executada.  
**Executor previsto:** GLM-5 via OpenClaude.  
**Diretório do componente:** `C:\workspaces\career-radar` (proposto; confirmar disponibilidade antes de criar).  
**Fontes de projetos:** `C:\workspaces\`.  
**Interface:** dashboard local + exportações HTML/Markdown.  
**Agendamento:** Windows Task Scheduler.  
**Persistência:** SQLite.  
**Escopo da primeira versão:** Marketing Intelligence e Career Radar.

---

## 1. Objetivo

Construir um sistema local, incremental e auditável que:

1. Indexe seletivamente projetos de `C:\workspaces\`, sem enviar repositórios inteiros ao modelo.
2. Reaproveite o kit de marketing existente da B. Industries.
3. Gere briefings, estudos de caso, pautas e rascunhos de conteúdo apoiados em evidências.
4. Descubra vagas por fontes públicas e integrações permitidas, deduplique e classifique oportunidades.
5. Compare requisitos de vagas com o perfil profissional e evidências reais.
6. Prepare pacotes de candidatura para revisão humana.
7. Apresente tudo em dashboard local, com fila, histórico, links, fontes, estados e logs.
8. Execute rotinas recorrentes via Windows Task Scheduler sem depender de uma sessão interativa aberta.
9. Respeite limites de custo/token, evite chamadas desnecessárias e recupere após falhas.

## 2. Princípios obrigatórios

- **Auditar antes de implementar:** examinar o repositório B. Industries e os materiais existentes antes de criar funcionalidades duplicadas.
- **Não inventar:** nenhuma experiência, cargo, certificação, métrica, funcionalidade, teste ou resultado pode ser afirmado sem evidência.
- **Evidência rastreável:** afirmações importantes devem apontar para arquivo/caminho/trecho ou URL pública e data de coleta.
- **Indexação incremental:** usar hash/timestamp e dependências de resumos para reprocessar apenas o necessário.
- **Contexto sob demanda:** o modelo recebe apenas briefings e trechos relevantes, nunca o repositório completo por padrão.
- **Privacidade por padrão:** não indexar nem enviar `.env`, tokens, chaves, credenciais, dados pessoais desnecessários ou arquivos privados.
- **Human-in-the-loop:** nenhuma publicação, mensagem, candidatura ou alteração de produção sem aprovação explícita.
- **Sem automação proibida:** não automatizar likes, comentários, mensagens, scraping autenticado ou publicação no LinkedIn. Usar fontes públicas e APIs/feeds cuja utilização esteja permitida.
- **Sem mudanças destrutivas:** não sobrescrever, apagar, renomear ou publicar ativos existentes sem autorização explícita.
- **Falha visível:** toda falha deve aparecer no log e no dashboard; não mascarar erros como resultados vazios.
- **Implementação por entregas pequenas:** plano, mudança, testes, revisão de diff e registro antes da próxima tarefa.
- **Configuração sem segredos:** segredos fora do código, prompts versionados e logs.
- **Windows-first:** comandos e scripts precisam funcionar no Windows 10/11; documentar dependências e caminhos.
- **Desacoplamento inicial:** o MVP deve funcionar independentemente; integrar ao painel/OS da B. Industries só depois de testes e decisão explícita.

## 3. Escopo funcional do MVP

### 3.1 Project Intelligence Index

- Descobrir projetos e repositórios sob `C:\workspaces\`.
- Permitir exclusões e inclusões por configuração.
- Identificar stack, linguagens, dependências, documentação, testes, CI/CD, comandos e estado Git quando disponível.
- Ignorar `.git`, `node_modules`, `.venv`, `venv`, `dist`, `build`, caches, artefatos grandes, dependências vendorizadas e binários por padrão.
- Detectar e excluir arquivos potencialmente secretos antes da leitura ou envio ao modelo.
- Guardar metadados, hashes, resumos e referências em SQLite.
- Fazer busca textual local (SQLite FTS5, se disponível) e recuperar trechos relevantes.
- Tratar resumos como índices de navegação, nunca como prova definitiva.
- Permitir reindexação de um projeto ou arquivo sob demanda.

### 3.2 Marketing Intelligence

- Ler o kit existente antes de alterá-lo.
- Reutilizar calendário, rascunhos, identidade visual, capturas, comandos de pauta e métricas existentes.
- Gerar briefings semanais e sugestões de pauta baseadas em projetos com evidências.
- Produzir rascunhos para LinkedIn pessoal e B. Industries, quando o ângulo justificar as duas versões.
- Gerar estudo de caso, estrutura de carrossel e checklist de evidências como saídas opcionais.
- Verificar duplicação com conteúdo anterior.
- Fazer revisão técnica e editorial.
- Manter estados: ideia, em elaboração, em revisão, aprovado, publicado manualmente, arquivado.
- Não publicar nem interagir automaticamente com contas sociais.
- Não inventar métricas de alcance ou engajamento. Métricas são inseridas manualmente pelo proprietário ou identificadas como indisponíveis.

### 3.3 Career Radar

- Coletar vagas de fontes públicas permitidas, páginas de carreiras, ATS públicos e fontes com API/feed autorizados.
- Começar com adaptadores simples e fontes explicitamente configuradas; não construir scraping genérico irrestrito.
- Usar alertas de plataformas como complemento quando a coleta direta não for apropriada.
- Guardar URL original, fonte, título, empresa, localidade, modalidade, data de publicação se disponível, data de coleta e descrição.
- Deduplicar por URL normalizada e sinais secundários.
- Marcar vagas fechadas ou expiradas apenas quando houver evidência suficiente.
- Filtrar por localização, modalidade, senioridade e famílias de função configuráveis.
- Comparar requisitos com um perfil profissional controlado e evidências do índice.
- Produzir pontuação explicável, com evidências, lacunas e incertezas.
- Preparar currículo adaptado, carta opcional, matriz de requisitos e checklist.
- Não enviar candidaturas automaticamente.

### 3.4 Dashboard e relatórios

- Dashboard somente local, vinculado a loopback por padrão; sem exposição pública.
- Visões: resumo, vagas, detalhes da vaga, marketing, fila de revisão, projetos indexados, execuções e configurações.
- Filtros por estado, data, fonte, pontuação e projeto.
- Ações locais para aprovar/rejeitar rascunhos, marcar vaga, adicionar motivo e exportar artefatos.
- Relatórios HTML/Markdown e exportação estruturada JSON/CSV quando útil.
- Histórico e logs com status, duração, erros, número de itens e uso estimado/real de tokens se disponível.

### 3.5 Agendamento

- Comandos CLI explícitos para indexação, coleta de vagas, análise, geração de conteúdo, dashboard e diagnósticos.
- Tarefas do Windows Task Scheduler chamam o runtime diretamente.
- Execução não interativa; não depender de terminal do OpenClaude aberto.
- Bloqueio contra execuções concorrentes, timeout, tentativas limitadas e logs rotativos.
- Modo `dry-run` para validar sem gravar estado final.
- A opção de retomar após falha do Windows pode ser documentada; o computador precisa estar ligado/acordado para executar.

## 4. Fora do escopo do MVP

- Publicação automática em LinkedIn ou outras redes.
- Likes, comentários, mensagens ou networking automatizados.
- Envio automático de candidaturas.
- Scraping atrás de login, bypass de CAPTCHA ou evasão de limites.
- Treinamento/fine-tuning de modelo.
- Indexar e enviar todos os tokens de todos os repositórios ao GLM-5.
- Deploy público do dashboard.
- Integração obrigatória com CRM, e-mail ou outros serviços externos.
- Vetores/embeddings antes de demonstrar que busca textual e recuperação por metadados são insuficientes.
- Migração do kit de marketing para outra estrutura antes da auditoria.

---

## 5. Decisões técnicas iniciais

- Linguagem: Python, versão compatível confirmada durante auditoria.
- Persistência: SQLite.
- Busca: SQLite FTS5 quando disponível; fallback explícito caso não esteja habilitado.
- Interface: dashboard web local simples, com tecnologia escolhida após verificar dependências existentes e compatibilidade Windows.
- Relatórios: HTML e Markdown.
- Testes: pytest ou framework já adotado pelo projeto, após auditoria.
- Configuração: TOML/YAML; escolher um único formato para o MVP.
- Modelo: adaptador isolado para GLM-5 via OpenClaude/9Router, somente depois de verificar um caminho de chamada não interativo e seus limites reais.
- Não assumir endpoint, autenticação, limite de contexto, preço ou nome exato de modelo sem verificar a configuração real.
- Se não houver interface programática estável, registrar bloqueio e apresentar alternativas verificadas; não simular integração funcional.

### Orçamento de tokens e contexto

- Definir limites configuráveis de tokens/chamadas por tarefa e por dia.
- Registrar consumo quando o provedor disponibilizar dados; caso contrário, registrar estimativa identificada como estimativa.
- Cachear saídas por hash do prompt, modelo, versão do prompt e hashes das evidências.
- Limitar o tamanho do briefing e recuperar trechos adicionais somente se faltar evidência.
- Usar funções determinísticas para hashes, datas, filtros, deduplicação, ranking inicial e relatórios.
- O modelo deve retornar saída estruturada validável.
- Em erros de formato, permitir no máximo uma tentativa de reparo automática por etapa, depois registrar a falha.
- Não colocar segredos ou conteúdo excluído no cache de prompts.

---

## 6. Plano de execução em prompts sequenciais

**Regra de uso:** enviar um prompt de cada vez ao GLM-5/OpenClaude. Só avançar após conferir os critérios de aceite. O agente deve relatar arquivos alterados, comandos executados, resultados de testes e riscos. Se uma tarefa revelar um bloqueio, parar e reportar em vez de improvisar uma integração.

### Prompt 0 — Auditoria e relatório de estado

```text
Você é o executor de engenharia do MVP Career Radar + Marketing Intelligence para a B. Industries. Nesta tarefa, NÃO implemente funcionalidades ainda.

Audite:
1. C:\workspaces\ e os projetos imediatamente abaixo, sem varrer recursivamente dependências ou artefatos enormes.
2. O repositório B. Industries, incluindo comercial/marketing/README.md, rascunhos, calendário, assets, scripts npm relacionados a marketing, métricas, demos e documentação.
3. A configuração disponível do GLM-5 via OpenClaude/9Router: nome/ID real do modelo, método de chamada não interativa disponível, variáveis de ambiente exigidas, limites e comportamento de erros que possam ser verificados sem revelar segredos.
4. O ambiente Windows/Python e dependências já instaladas.
5. Existência de C:\workspaces\career-radar; se existir, inspecione e preserve.

Não leia, imprima, copie ou envie segredos. Nunca exiba valores de chaves/tokens; apenas informe presença/ausência e nomes das variáveis. Não altere arquivos, instale pacotes, faça deploy ou crie tarefas agendadas.

Entregue:
- inventário conciso de repositórios;
- mapa do kit de marketing existente e comandos reais;
- funcionalidades que devem ser reutilizadas;
- opções verificadas para chamadas não interativas ao modelo;
- riscos, lacunas e perguntas bloqueantes;
- arquitetura recomendada e árvore de arquivos proposta;
- plano de execução em entregas pequenas;
- comandos exatos para reproduzir a auditoria.

Diferencie fatos verificados, inferências e informações desconhecidas. Não invente caminhos, comandos, APIs ou limites.
```

**Critérios de aceite**
- Nenhum arquivo modificado.
- Kit de marketing descrito com caminhos e comandos realmente encontrados.
- Integração do modelo classificada como verificada, não verificada ou bloqueada.
- Nenhum segredo impresso ou enviado.
- Arquitetura sem duplicar funcionalidades existentes.

**Testes/verificações**
- `git status --short` antes/depois nos repositórios examinados, quando Git estiver disponível.
- Conferir se os comandos citados existem nos manifests/documentação.
- Conferir que nenhuma instalação ou alteração foi realizada.

### Prompt 1 — Especificação técnica e contrato de dados

```text
Com base no relatório de auditoria aprovado, produza a especificação técnica do MVP. Não implemente ainda.

Defina:
- módulos e responsabilidades;
- esquema SQLite e migrações;
- entidades: Project, FileRecord, Evidence, PromptRun, JobSource, JobPosting, JobMatch, ContentDraft, ReviewAction, RunLog;
- estados e transições válidas;
- configuração de exclusões/inclusões;
- contrato de adaptador de modelo;
- contrato dos adaptadores de fontes de vagas;
- contrato de relatórios e dashboard;
- política de cache, limites de tokens, timeout e retentativas;
- modelo de segurança e privacidade;
- critérios de aceite por módulo.

Ajuste nomes/campos após verificar a implementação existente. Não duplique tabelas ou estruturas de um componente existente sem justificativa. Identifique decisões arquiteturais como ADRs. Não invente suporte de provedor nem de fonte de vagas.
```

**Critérios de aceite**
- Esquema documentado e versionável.
- Estados e relações sem ambiguidades.
- Contratos permitem trocar provedor de IA e fontes sem reescrever o núcleo.
- Cada requisito é rastreável a teste.
- Bloqueios e decisões pendentes estão explicitados.

**Testes/verificações**
- Revisão de consistência do esquema e estados.
- Checagem de que todos os requisitos do escopo aparecem na matriz de aceite.
- Nenhum código funcional alterado nesta etapa.

### Prompt 2 — Esqueleto do projeto, configuração e banco

```text
Implemente somente o esqueleto aprovado do Career Radar em C:\workspaces\career-radar, se a auditoria confirmou que esse destino é seguro e está livre ou já contém um projeto compatível.

Crie estrutura mínima, CLI, configuração de exemplo sem segredos, logging, SQLite, migrações versionadas e testes básicos. Não copie dados privados dos outros projetos. Não altere o repositório B. Industries. Se o destino já tiver conteúdo, preserve e adapte em vez de sobrescrever.

Inclua comandos CLI de diagnóstico e status. A CLI deve funcionar sem chamada ao modelo. Configure paths relativos à raiz do projeto quando possível e valide caminhos absolutos. Não crie ainda o dashboard, fontes de vagas ou geração editorial.
```

**Critérios de aceite**
- Inicialização e status funcionam sem rede e sem chave de IA.
- Banco criado de forma idempotente.
- Configuração inválida gera erro compreensível.
- Nenhum segredo está no repositório.
- Testes básicos passam.

**Testes/verificações**
- Testes em banco temporário.
- Duas inicializações seguidas sem duplicar schema.
- Testes de configuração ausente, inválida e com campos opcionais.
- `git diff --check` e revisão do diff.

### Prompt 3 — Verificação do adaptador GLM-5 / OpenClaude / 9Router

```text
Implemente o adaptador de IA apenas com base no método não interativo confirmado pela auditoria. Não presuma endpoint, nome de modelo, formato de autenticação ou contexto.

Requisitos:
- interface única para gerar saída;
- timeout, tratamento de rate limit, falha de rede, erro de autenticação e resposta malformada;
- parâmetros configuráveis para modelo e limites;
- saída estruturada validada por schema;
- registro de modelo/versão de prompt/duração/uso reportado pelo provedor, quando disponível;
- segredos somente via ambiente ou mecanismo seguro confirmado;
- logs sanitizados;
- modo mock para testes;
- cache opcional por hash de entrada não sensível, versão do prompt e modelo;
- limite de chamadas e tokens por tarefa/dia quando a métrica for disponível.

Faça um teste mínimo e explícito de conectividade usando uma entrada sintética sem dados pessoais. Nunca registre chave/token ou cabeçalhos secretos. Se a integração não puder ser chamada de modo não interativo, não simule sucesso: registre o bloqueio e proponha alternativas verificáveis.
```

**Critérios de aceite**
- Testes não precisam de acesso real ao provedor.
- Uma chamada sintética real funciona, se permitida pela configuração.
- Falhas não viram texto vazio ou sucesso falso.
- Segredos não aparecem nos logs.
- Limites de contexto/custo não são inventados.

**Testes/verificações**
- Mock para sucesso, timeout, rate limit, autenticação falha e JSON inválido.
- Verificação de sanitização de logs.
- Teste real opcional e explicitamente identificado, com custo esperado informado antes de executar.

### Prompt 4 — Índice incremental de `C:\workspaces\`

```text
Implemente o indexador incremental conforme a especificação aprovada.

O indexador deve:
- descobrir projetos por marcadores de repositório e configuração;
- suportar inclusões/exclusões explícitas;
- ignorar dependências, builds, caches, binários e diretórios configurados;
- detectar nomes e padrões de segredos antes de ler conteúdo para indexação;
- não seguir junctions/symlinks fora da raiz sem opção explícita;
- armazenar caminho relativo/normalizado, tamanho, mtime, hash e estado;
- extrair metadados de arquivos de texto permitidos;
- permitir indexar projeto único, arquivo único, incremental e rebuild explícito;
- identificar arquivos removidos sem apagar o histórico de forma irrecuperável;
- suportar limite de tamanho por arquivo e por projeto;
- nunca enviar arquivo ao modelo durante a varredura inicial.

Comece por uma amostra configurada, não por toda a árvore, até que o relatório de exclusões seja revisado. Inclua dry-run que liste o que seria indexado/excluído.
```

**Critérios de aceite**
- Primeira execução produz inventário reproduzível.
- Segunda execução sem mudanças não reprocessa conteúdo desnecessário.
- Mudança de arquivo atualiza hash e invalida apenas resumos dependentes.
- Arquivos ignorados e motivos são auditáveis.
- Junctions e caminhos externos não são seguidos por padrão.
- Nenhum conteúdo é enviado à IA pela indexação básica.

**Testes/verificações**
- Fixture com diretórios permitidos/proibidos, arquivos grandes, binários, segredos fictícios e symlinks.
- Testes para criação, alteração, remoção e renomeação.
- Teste de dry-run.
- Comparar contagem de arquivos elegíveis com resultado esperado.

### Prompt 5 — Extração de evidências, resumos e recuperação

```text
Implemente a camada de evidências sobre o índice existente.

Extraia metadados determinísticos de manifests, READMEs, documentação, testes, CI e configuração de dependências, usando parsers seguros e limites de tamanho. Armazene caminho, linha/trecho quando disponível, hash de origem, tipo de evidência e data de extração.

Gere resumos de projeto somente depois de compilar evidências locais. O prompt deve instruir o modelo a distinguir implementado, configurado, planejado, demonstrado e desconhecido. Cada afirmação relevante precisa de referências a evidências. Se não houver suporte, marque como desconhecido.

Implemente busca textual local e montagem de briefing com orçamento de contexto configurável. Não implemente embeddings até que uma avaliação mostre que a busca textual é insuficiente. Permita abrir/verificar o arquivo original. Não tratar resumo antigo como verdade após alteração da fonte.
```

**Critérios de aceite**
- Busca retorna caminho e trecho de origem.
- Resumo diferencia fatos e incertezas.
- Alteração de arquivo invalida a evidência/resumo derivado.
- Briefing respeita limite configurado e registra itens omitidos.
- Nenhum arquivo inteiro é enviado sem seleção explícita.

**Testes/verificações**
- Casos conhecidos com respostas esperadas.
- Testes de rastreabilidade de afirmações.
- Testes de limite de briefing e de invalidação.
- Teste de ausência de evidência.

### Prompt 6 — Marketing Intelligence

```text
Integre Marketing Intelligence ao kit existente da B. Industries sem substituir nem reorganizar seus materiais.

Antes de implementar, confirme os caminhos e comandos reais encontrados na auditoria. Reutilize, quando compatível, o calendário editorial, rascunhos, assets, capturas, rotina de pauta e medição.

Implemente um fluxo:
1. escolher projeto/público/objetivo;
2. recuperar evidências verificáveis;
3. gerar briefing editorial;
4. gerar uma pauta e rascunhos distintos para perfil pessoal e empresa quando apropriado;
5. opcionalmente gerar outline de carrossel e estudo de caso;
6. verificar duplicação com histórico;
7. revisar tecnicamente afirmações e links;
8. produzir checklist de evidências e fila de aprovação;
9. exportar HTML/Markdown e registrar versão/status.

Mantenha o tom técnico, específico e honesto. Não use clichês vazios, não invente números ou resultados, não transforme plano em funcionalidade implementada e não declare que uma demo guiada contém IA ativa se não contiver. Não publique nem acesse contas sociais automaticamente. Métricas devem vir de entrada manual ou aparecer como indisponíveis.
```

**Critérios de aceite**
- Kit existente preservado.
- Rascunhos citam evidências e distinguem limitações.
- Conteúdos novos são comparados ao histórico.
- Aprovação e publicação manual são estados separados.
- Métricas ausentes não são fabricadas.
- Exportação é reproduzível e não vaza dados internos.

**Testes/verificações**
- Fixture de projeto com capacidades reais e planejadas.
- Caso sem métricas.
- Caso com alegação não suportada: revisão deve bloquear ou sinalizar.
- Teste de duplicação aproximada.
- Teste de exportação e estados.

### Prompt 7 — Fontes públicas e coleta de vagas

```text
Implemente o Career Radar começando por adaptadores de fontes públicas explicitamente configuradas e permitidas. A auditoria deve identificar fontes e interfaces reais antes da implementação.

Priorize fontes com API/feed documentado, ATS públicos e páginas de carreira que permitam coleta razoável. Respeite termos, robots/políticas aplicáveis, limites de requisição e cache. Não faça login automatizado, não contorne CAPTCHA e não faça scraping proibido. LinkedIn pode ser usado via alertas fornecidos pelo usuário ou interfaces oficialmente permitidas, mas não automatize atividade da conta.

Normalize os dados e guarde fonte, URL original, data de coleta, data de publicação quando disponível, descrição, localidade, modalidade, empresa e status de verificação. Implemente deduplicação por URL e sinais secundários. Se uma fonte falhar, registre a falha separadamente de “nenhuma vaga encontrada”. Não invente salário nem data de publicação.
```

**Critérios de aceite**
- Cada vaga tem fonte e URL rastreáveis.
- Duplicatas são controladas sem apagar a origem.
- Erros de fonte aparecem no dashboard/log.
- Ausência de salário/data é representada como desconhecida.
- Fontes não permitidas são excluídas do escopo.

**Testes/verificações**
- Fixtures locais de respostas válidas, incompletas, duplicadas e inválidas.
- Teste de rate limit e timeout com mocks.
- Teste de normalização de URL.
- Teste de falha de fonte versus lista vazia.
- Teste de atualização/expiração com evidência.

### Prompt 8 — Perfil, filtros, pontuação e explicações

```text
Implemente o perfil profissional controlado e o ranking de vagas.

O perfil deve ter campos editáveis e origem identificável: experiência, competências, projetos, links, localização/modalidade preferidas, senioridade desejada, restrições obrigatórias e tipos de vaga priorizados. Não presumir salário mínimo, modalidade ou senioridade; usar “não configurado” até confirmação.

Aplique filtros determinísticos para restrições obrigatórias. Para vagas restantes, produza uma pontuação explicável com pesos configuráveis. Valores iniciais sugeridos: compatibilidade técnica demonstrada 30; responsabilidades 20; senioridade 15; localização/modalidade 15; afinidade de área 10; qualidade/atualidade dos dados 10. Trate esses pesos como hipótese a calibrar.

A saída deve listar correspondências fortes, parciais, lacunas, desconhecidos, evidências do perfil e evidências da descrição da vaga. Não inferir experiência profissional só porque um pacote aparece num repositório. Não apresentar a nota como probabilidade de contratação.
```

**Critérios de aceite**
- Filtros eliminatórios são aplicados antes do ranking.
- Cada pontuação tem explicação e pesos visíveis.
- Lacunas e dados ausentes são explícitos.
- Perfil pode ser editado sem alterar o código.
- Não há afirmações profissionais sem evidência.

**Testes/verificações**
- Vaga claramente compatível, parcialmente compatível e incompatível.
- Requisito eliminatório não configurado.
- Testes de soma/pesos e ordenação.
- Teste para distinguir uso de tecnologia em projeto de experiência profissional.

### Prompt 9 — Pacote de candidatura

```text
Implemente a preparação assistida de candidaturas para vagas selecionadas.

Produza:
- resumo da vaga;
- matriz de requisitos e evidências;
- riscos/lacunas;
- currículo adaptado a partir de um perfil-base confirmado;
- carta opcional;
- respostas sugeridas para perguntas, marcando itens que exigem confirmação;
- links para vaga e projetos;
- checklist final.

Não crie experiência, cargos, datas, resultados, certificações ou métricas. Preserve o currículo-base e salve uma nova versão identificada por vaga/data. Toda saída permanece como rascunho; não enviar candidaturas. Ofereça exportação HTML/Markdown e, se bibliotecas existentes permitirem, DOCX/PDF como etapa opcional sem bloquear o MVP.
```

**Critérios de aceite**
- Currículo-base nunca é sobrescrito.
- Toda afirmação nova é suportada ou marcada para confirmação.
- Pacote aponta para vaga e evidências.
- Nenhuma candidatura é enviada.
- Histórico preserva a versão usada.

**Testes/verificações**
- Perfil com lacunas deliberadas.
- Vaga com requisitos não atendidos.
- Verificar que cargo/anos/resultados não aparecem sem fonte.
- Teste de exportação e nomes de arquivos seguros.

### Prompt 10 — Dashboard local e fila de revisão

```text
Implemente um dashboard local simples usando a stack escolhida após a auditoria.

Inclua:
- resumo de execuções e erros;
- vagas novas e ordenadas, filtros e detalhes;
- fontes e links originais;
- evidências e lacunas da análise;
- ações para marcar interessante, descartar com motivo, preparar candidatura e registrar status;
- fila de rascunhos de marketing com evidências, edição, aprovação/rejeição e exportação;
- projetos indexados e estado do índice;
- histórico de tarefas, duração, chamadas e erros;
- tela de configuração sem exibir segredos.

Vincule a interface a loopback por padrão e não exponha à rede. Não implemente login remoto porque o dashboard é local e single-user; ainda assim, não aceite conexões externas por padrão. Ações destrutivas exigem confirmação. Nunca inclua botões de publicação/envio automático.
```

**Critérios de aceite**
- Dashboard abre localmente e não fica exposto à LAN por padrão.
- Estados são persistidos no SQLite.
- Ações são auditadas e reversíveis quando possível.
- Erros e fontes indisponíveis ficam visíveis.
- Não há publicação ou candidatura automática.

**Testes/verificações**
- Testes de rotas e operações do banco.
- Verificação de endereço de bind.
- Teste de transições de estado e motivos de descarte.
- Teste de escape/segurança para texto de vaga e conteúdo gerado.
- Teste de reinicialização preservando histórico.

### Prompt 11 — Agendamento e operação Windows

```text
Implemente comandos CLI para: doctor/status, index incremental, coletar vagas, analisar vagas, gerar pauta, exportar relatórios e abrir o dashboard. Confirme os nomes finais antes de criar tarefas.

Crie scripts PowerShell documentados para registrar/remover tarefas do Windows Task Scheduler, mas NÃO registre tarefas automaticamente sem confirmação explícita. As tarefas devem executar o Python/venv de forma não interativa, com diretório de trabalho explícito, logs, política de concorrência, timeout e tratamento de falhas.

Sugira inicialmente:
- indexação incremental diária;
- Career Radar diário em dia/horário configurável;
- Marketing Intelligence semanal.
Os horários devem ser configuráveis. Evite duas tarefas que alterem o mesmo estado simultaneamente. Documente que a máquina precisa estar ligada/acordada e como configurar retomada após suspensão, se desejado.
```

**Critérios de aceite**
- Todos os comandos funcionam manualmente.
- Scripts de agendamento são idempotentes e mostram o que farão.
- Nenhuma tarefa é registrada sem aprovação.
- Logs são rotacionados e falhas não desaparecem.
- Execuções concorrentes são bloqueadas ou serializadas.

**Testes/verificações**
- Execução manual e `dry-run`.
- Teste de lock.
- Teste de timeout e código de saída não zero.
- Validar comandos PowerShell sem registrar tarefa real inicialmente.
- Verificar working directory e caminhos com espaços.

### Prompt 12 — Testes integrados, avaliação de qualidade e hardening

```text
Faça uma revisão completa do MVP implementado, sem adicionar funcionalidades grandes.

Execute testes unitários, integração e smoke tests. Crie fixtures de projetos e vagas representativas, incluindo fontes incompletas, dados duplicados, alegações sem evidência, perfil incompleto e falhas de provedor.

Audite:
- segredos e dados sensíveis;
- caminhos fora da raiz;
- arquivos excluídos;
- chamadas de IA e limites;
- logs;
- duplicação;
- rastreabilidade das afirmações;
- tratamento de erros;
- dashboard local;
- estados de aprovação;
- preservação dos materiais B. Industries;
- documentação e instalação no Windows.

Não faça deploy, não publique posts, não envie candidaturas e não registre tarefas reais sem autorização. Não declare testes aprovados se não foram executados. Corrija apenas defeitos dentro do escopo e mostre o diff final.
```

**Critérios de aceite**
- Testes executados e resultados reportados com comandos.
- Falhas restantes explicitadas.
- `git diff --check` passa, se Git estiver disponível.
- Nenhum segredo ou dado sensível em logs/fixtures.
- Nenhuma publicação, candidatura ou tarefa real feita sem aprovação.
- Guia de instalação, operação, backup e recuperação atualizado.

### Prompt 13 — Ensaio operacional antes da ativação

```text
Execute um ensaio em modo dry-run com uma pequena amostra de projetos e fontes de vagas. Use apenas conteúdo permitido e fontes configuradas.

Gere um relatório de avaliação com:
- projetos e arquivos elegíveis/excluídos e motivos;
- evidências encontradas;
- quantidade de vagas coletadas, duplicadas e descartadas;
- exemplos de explicações de ranking;
- rascunhos de marketing com checklist de afirmações;
- latência, erros e consumo de IA quando mensurável;
- itens que exigem confirmação do proprietário;
- recomendação de ativação ou bloqueios.

Não ative tarefas agendadas nem faça publicações/candidaturas. Pare se detectar exposição de segredo, varredura fora da raiz ou chamadas ao provedor não previstas.
```

**Critérios de aceite**
- Resultados podem ser reproduzidos.
- Todas as afirmações de exemplo são rastreáveis.
- Custos/uso são medidos ou explicitamente marcados como não disponíveis.
- Bloqueios são documentados.
- Ativação só ocorre após revisão humana.

---

## 7. Modelo de dados conceitual

O esquema definitivo deve ser ajustado após a auditoria. Entidades mínimas esperadas:

- `projects`: nome, caminho, tipo, branch/commit se disponível, estado de indexação.
- `files`: caminho relativo, tamanho, mtime, hash, tipo, elegibilidade e motivo de exclusão.
- `evidence`: projeto/arquivo, localização, trecho, categoria, hash de origem, data.
- `project_summaries`: versão, resumo, referências e hashes das evidências usadas.
- `prompt_runs`: prompt/versionamento, modelo, hash de entrada, estado, duração, uso informado/estimado.
- `job_sources`: nome, método, configuração não secreta, estado e última coleta.
- `job_postings`: URL normalizada, fonte, empresa, título, descrição, datas e estado.
- `job_matches`: vaga, versão de perfil, nota, pesos, evidências, lacunas e incertezas.
- `candidate_profiles`: dados profissionais confirmados e preferências.
- `application_packages`: vaga, versão de currículo, carta, checklist e estado.
- `content_drafts`: projeto, canal, público, texto, referências, versão de prompt e estado.
- `review_actions`: ator local, ação, data, motivo e entidade.
- `run_logs`: tipo de execução, início/fim, status, contagens, erro sanitizado.

Não armazenar segredos, cabeçalhos de autenticação ou dados desnecessários. Criar índices para URL normalizada, data, estado e chaves de relação. Usar transações e migrações versionadas.

---

## 8. Ranking inicial das vagas

Ponto de partida configurável, não calibrado:

| Critério | Peso inicial |
|---|---:|
| Compatibilidade técnica demonstrada | 30 |
| Correspondência com responsabilidades | 20 |
| Senioridade/experiência solicitada | 15 |
| Localização e modalidade | 15 |
| Afinidade com áreas priorizadas | 10 |
| Qualidade/atualidade dos dados | 10 |
| **Total** | **100** |

Requisitos eliminatórios configurados pelo usuário devem ser filtros, não apenas penalidades. A nota não é uma probabilidade de contratação. O relatório deve mostrar correspondências, lacunas, desconhecidos e evidências.

Famílias de vaga iniciais a considerar: Java/Spring Boot, Python/backend, TypeScript/React/Next.js, APIs/integrações, DevOps/infra/cloud, automação e IA aplicada. A prioridade real deve ser configurável, não presumida como definitiva.

---

## 9. Rotinas agendadas propostas

Os horários são exemplos; configurar depois de validar duração, disponibilidade e custo.

1. **Indexação incremental diária:** antes das análises, se houver alterações.
2. **Career Radar diário:** coletar, normalizar, deduplicar, filtrar, analisar e atualizar dashboard.
3. **Marketing Intelligence semanal:** escolher pautas, preparar rascunhos, revisar e alimentar fila.
4. **Diagnóstico semanal:** verificar últimas execuções, erros, fontes sem atualização e uso de IA.

Regras:
- Um processo não pode executar duas vezes em paralelo sobre o mesmo estado.
- Falha em uma fonte não deve impedir as demais.
- Repetições são limitadas e respeitam `Retry-After`/limites da fonte quando aplicável.
- O agendamento não deve abrir OpenClaude em modo interativo.
- A ativação das tarefas reais requer confirmação humana.
- O dashboard é local e não é hospedado publicamente no MVP.

---

## 10. Critérios globais de “pronto”

O MVP só é considerado pronto para uso recorrente quando:

1. O índice incremental funciona com fixtures e uma amostra real aprovada.
2. Exclusões e segredos são testados.
3. A integração de IA funciona sem sessão interativa ou há um bloqueio explicitamente resolvido.
4. O sistema respeita limites configurados e registra falhas.
5. O marketing reaproveita o kit existente e gera rascunhos rastreáveis.
6. O Career Radar encontra e deduplica vagas de fontes configuradas e permitidas.
7. O ranking mostra evidências, lacunas e incertezas.
8. O pacote de candidatura não inventa dados.
9. O dashboard local preserva histórico e estados.
10. Os relatórios HTML/Markdown podem ser exportados.
11. Os comandos manuais funcionam antes do agendamento.
12. O Task Scheduler só é ativado depois do ensaio e da aprovação.
13. Nenhuma publicação ou candidatura é automática.
14. Documentação inclui setup Windows, configuração, execução manual, logs, backup, restauração e solução de problemas.

## 11. Checklist de revisão humana antes da ativação

- [ ] Auditoria do kit existente revisada.
- [ ] Caminho de destino confirmado.
- [ ] Método não interativo do modelo confirmado.
- [ ] Fontes públicas e políticas de acesso revisadas.
- [ ] Exclusões do indexador revisadas.
- [ ] Perfil profissional revisado e confirmado.
- [ ] Preferências de vaga obrigatórias configuradas.
- [ ] Resultados de ranking avaliados com exemplos.
- [ ] Rascunhos de marketing revisados.
- [ ] Dashboard vinculado somente a loopback.
- [ ] Limites de custo/token configurados.
- [ ] Testes e ensaio dry-run concluídos.
- [ ] Tarefas do Windows Task Scheduler revisadas antes de registrar.
- [ ] Backup e recuperação documentados.

## 12. Instrução operacional final ao executor

Trabalhe estritamente na ordem dos prompts. Não avance automaticamente para o próximo prompt. Ao final de cada tarefa, entregue:
1. resumo do que foi feito;
2. arquivos criados/alterados;
3. comandos executados;
4. testes e resultados reais;
5. riscos e bloqueios;
6. decisões que precisam de aprovação;
7. prompt sugerido para a próxima tarefa.

Se o contexto ficar grande, pare e produza um resumo de estado factual com caminhos, decisões, testes e tarefas pendentes. Não recomece a auditoria inteira sem necessidade. Nunca afirme que algo foi testado ou executado se não foi.
