# Feature Specification: Planejador BNCC para Professores

**Feature Branch**: `docs/especificacao` (branch atual; não alterada)

**Created**: 2026-10-02

**Status**: Draft

**Input**: User description: "Planejador BNCC para professores com autenticação por contas de
demonstração, consulta de habilidades, geração assistida por IA e rascunhos privados editáveis."

## Clarifications

### Session 2026-10-02

- Q: Quando a sessão do professor deve expirar por inatividade? → A: Expirar após 30 minutos de inatividade.
- Q: Quais elementos mínimos uma resposta da IA deve conter para ser aceita como um plano válido? → A: Título, objetivos, desenvolvimento e avaliação em Markdown.
- Q: Se o mesmo professor editar o mesmo rascunho em duas sessões e salvar versões diferentes, como o sistema deve agir? → A: Informar conflito e exigir recarregar antes de salvar.
- Q: Após quanto tempo sem resposta a geração por IA deve ser considerada falha? → A: Após 60 segundos sem resposta.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Gerar um rascunho de plano (Priority: P1)

Um professor autenticado pesquisa e seleciona habilidades BNCC, informa a orientação pedagógica,
duração e uso de recursos digitais e solicita um plano. Após um estado de preparação, ele recebe um
rascunho privado com o conteúdo gerado e a indicação de auxílio por IA.

**Why this priority**: Esta é a entrega central: transformar a seleção curricular do professor em
um plano revisável.

**Independent Test**: Com uma conta de demonstração, selecionar ao menos uma habilidade, preencher
os campos obrigatórios e concluir uma geração válida; o professor vê um rascunho privado em Markdown.

**Acceptance Scenarios**:

1. **Given** um professor autenticado e uma ou mais habilidades selecionadas, **When** informa uma
   instrução pedagógica, duração em minutos e a opção de recursos digitais e confirma, **Then** vê
   um estado de preparação até a conclusão da solicitação.
2. **Given** uma resposta válida da IA, **When** a preparação é concluída, **Then** é criado um plano
   privado no estado RASCUNHO, associado ao professor, com a indicação de auxílio por IA e conteúdo
   em Markdown.
3. **Given** o rascunho gerado, **When** o professor abre o plano, **Then** vê o Markdown completo e
   uma visualização de sua apresentação.
4. **Given** uma resposta da IA sem título, objetivos, desenvolvimento ou avaliação, **When** o
   resultado é validado, **Then** a geração falha e nenhum rascunho é criado.

---

### User Story 2 - Acessar o ambiente com conta de demonstração (Priority: P1)

Um professor inicia sessão usando uma das contas de demonstração previamente cadastradas, acessa o
próprio ambiente e pode encerrar a sessão.

**Why this priority**: A identificação do professor é necessária para proteger os planos e permitir
testes de acesso privado.

**Independent Test**: Entrar com cada uma das duas contas fornecidas, confirmar a sessão ativa e
encerrar a sessão; credenciais inválidas não concedem acesso.

**Acceptance Scenarios**:

1. **Given** uma conta de demonstração válida, **When** o professor informa suas credenciais, **Then**
   inicia uma sessão e chega ao ambiente de planos.
2. **Given** credenciais inválidas, **When** tenta entrar, **Then** recebe uma mensagem de erro e não
   inicia sessão.
3. **Given** uma sessão ativa, **When** o professor faz logout, **Then** a sessão é encerrada e áreas
   privadas deixam de estar acessíveis.

---

### User Story 3 - Localizar e selecionar habilidades BNCC (Priority: P1)

Um professor consulta o catálogo mínimo de habilidades BNCC por nível, ano quando aplicável, eixo,
código ou trecho de texto e seleciona uma ou mais habilidades para a solicitação do plano.

**Why this priority**: A seleção curricular fundamenta a utilidade pedagógica do plano gerado.

**Independent Test**: Pesquisar habilidades pelos cinco critérios suportados e incluir/remover mais
de uma habilidade antes da geração.

**Acceptance Scenarios**:

1. **Given** o catálogo mínimo disponível, **When** o professor pesquisa por nível, **Then** vê apenas
   habilidades daquele nível.
2. **Given** uma habilidade com ano aplicável, **When** filtra por ano, eixo, código ou texto, **Then**
   os resultados atendem ao critério informado.
3. **Given** resultados de pesquisa, **When** seleciona ou remove habilidades, **Then** a seleção atual
   reflete exatamente as habilidades escolhidas.

---

### User Story 4 - Revisar e salvar o rascunho (Priority: P2)

Um professor edita o Markdown de seu rascunho, atualiza a visualização e salva a alteração de modo
explícito. Também consulta a lista de seus rascunhos.

**Why this priority**: O professor mantém controle editorial sobre conteúdo assistido por IA.

**Independent Test**: Abrir um rascunho próprio, alterar um trecho do Markdown, salvar e confirmar a
alteração ao reabrir o plano e na lista de rascunhos.

**Acceptance Scenarios**:

1. **Given** um rascunho do professor, **When** altera o Markdown sem salvar, **Then** a alteração não
   é tratada como persistida até que ele acione salvar explicitamente.
2. **Given** uma edição salva, **When** reabre o rascunho, **Then** vê o Markdown salvo e sua
   visualização correspondente.
3. **Given** vários rascunhos próprios, **When** consulta a lista, **Then** vê somente seus rascunhos.
4. **Given** um rascunho alterado e salvo em outra sessão do mesmo professor, **When** tenta salvar
   uma edição desatualizada, **Then** é informado do conflito e deve recarregar o rascunho antes de
   salvar novamente.

---

### User Story 5 - Recuperar-se de falha de geração (Priority: P2)

Se a geração não for concluída, o professor recebe uma mensagem compreensível, preserva os campos
informados e decide se deseja iniciar uma nova tentativa.

**Why this priority**: Falhas externas não podem apagar trabalho docente nem criar planos incompletos.

**Independent Test**: Simular uma resposta inválida ou indisponibilidade durante a geração e confirmar
que os dados continuam preenchidos, nenhum rascunho é criado e só uma ação explícita reinicia o fluxo.

**Acceptance Scenarios**:

1. **Given** uma falha ou resposta inválida durante a geração, **When** o processo termina, **Then** os
   campos e habilidades selecionadas permanecem disponíveis ao professor.
2. **Given** uma falha de geração, **When** o professor consulta seus rascunhos, **Then** nenhum plano
   parcial proveniente daquela solicitação aparece na lista.
3. **Given** uma falha exibida, **When** o professor não aciona nova tentativa, **Then** nenhuma nova
   solicitação de geração é iniciada.
4. **Given** uma solicitação de geração sem resposta por 60 segundos, **When** esse prazo é atingido,
   **Then** ela é tratada como falha, os campos são preservados e nenhum rascunho é criado.

---

### User Story 6 - Preservar a privacidade entre professores (Priority: P1)

Um professor só pode ler e editar os próprios rascunhos; a segunda conta de demonstração permite
validar que os dados permanecem privados.

**Why this priority**: Planos docentes são dados privados e a autorização por proprietário é uma regra
fundamental do produto.

**Independent Test**: Criar um rascunho com a primeira conta, entrar com a segunda e tentar acessar
o rascunho pela lista e por um endereço direto; o acesso é negado em ambos os casos.

**Acceptance Scenarios**:

1. **Given** um rascunho criado pela conta A, **When** a conta B consulta sua lista, **Then** o
   rascunho da conta A não é exibido.
2. **Given** um rascunho criado pela conta A, **When** a conta B tenta abri-lo ou alterá-lo diretamente,
   **Then** o acesso é negado e o conteúdo não é revelado nem modificado.

### Edge Cases

- O sistema impede a confirmação de geração sem ao menos uma habilidade selecionada, instrução
  pedagógica preenchida e duração positiva em minutos.
- Uma pesquisa sem resultados informa claramente a ausência de habilidades compatíveis e preserva os
  filtros aplicados.
- O catálogo pode conter habilidades sem ano aplicável; a ausência de ano não exclui essas habilidades
  quando o professor pesquisa por outros critérios.
- A expiração ou encerramento da sessão impede o acesso a listas, detalhes e edição de rascunhos até
  novo login; a sessão expira após 30 minutos de inatividade.
- Quando o mesmo rascunho for salvo em outra sessão antes de uma edição local ser salva, o sistema
  informa o conflito e exige recarregar antes de aceitar novo salvamento.
- Uma resposta de IA vazia, malformada ou incompatível com a solicitação é tratada como falha, sem
  persistir plano parcial; uma resposta válida contém título, objetivos, desenvolvimento e avaliação
  em Markdown.
- A ausência de resposta da IA por 60 segundos é tratada como falha de geração, sem persistir plano
  parcial nem reiniciar a solicitação automaticamente.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O sistema MUST disponibilizar exatamente duas contas de demonstração previamente
  cadastradas e não MUST oferecer cadastro público nem administração de contas.
- **FR-002**: O sistema MUST autenticar o professor por credenciais de demonstração, encerrar a sessão
  após 30 minutos de inatividade e permitir logout explícito.
- **FR-003**: O sistema MUST exigir autenticação para consultar, criar, visualizar, editar ou salvar
  planos.
- **FR-004**: O sistema MUST disponibilizar um catálogo mínimo de habilidades BNCC pesquisável por
  nível, ano quando aplicável, eixo, código e texto.
- **FR-005**: O professor MUST poder selecionar uma ou mais habilidades e remover qualquer seleção
  antes de solicitar a geração.
- **FR-006**: O sistema MUST coletar instrução pedagógica, duração positiva em minutos e indicação de
  uso de recursos digitais antes de aceitar uma solicitação válida.
- **FR-007**: Após uma solicitação válida, o sistema MUST apresentar um estado de preparação enquanto
  aguarda o resultado da geração.
- **FR-008**: O sistema MUST criar um plano somente após receber uma resposta de IA válida; o plano
  criado MUST ser privado, pertencer ao professor solicitante, ter estado RASCUNHO e indicar auxílio
  por IA. Uma resposta válida MUST conter título, objetivos, desenvolvimento e avaliação em Markdown.
- **FR-009**: O sistema MUST apresentar o conteúdo de cada rascunho em Markdown e uma visualização de
  apresentação desse conteúdo.
- **FR-010**: O professor MUST poder editar o Markdown de seu próprio rascunho e persistir mudanças
  somente após uma ação explícita de salvar.
- **FR-010a**: O sistema MUST rejeitar um salvamento baseado em uma versão desatualizada do rascunho,
  informar o conflito e exigir que o professor recarregue o conteúdo antes de salvar novamente.
- **FR-011**: O sistema MUST listar somente os rascunhos pertencentes ao professor autenticado.
- **FR-012**: O sistema MUST negar leitura e alteração de um rascunho a qualquer professor que não seja
  seu proprietário, inclusive por tentativa de acesso direto.
- **FR-013**: Em caso de falha ou resposta inválida da geração, o sistema MUST preservar os campos e a
  seleção atual, informar a falha, não criar plano parcial e não reiniciar a geração sem ação do
  professor. A ausência de resposta por 60 segundos MUST ser tratada como falha.
- **FR-014**: O escopo MUST excluir exportação em PDF, finalização de planos, versionamento de planos e
  publicação pública de planos.
- **FR-015**: As telas MUST usar componentes e tokens coerentes com o Design System de referência no
  Figma e permanecer utilizáveis em desktop, tablet e celular.

### Key Entities *(include if feature involves data)*

- **Professor**: Pessoa autenticada por uma conta de demonstração; é proprietária de seus planos.
- **Sessão**: Estado de acesso que identifica o professor autenticado até logout ou expiração.
- **Habilidade BNCC**: Item curricular do catálogo, identificado por código, texto, nível, eixo e,
  quando aplicável, ano.
- **Solicitação de plano**: Dados enviados pelo professor para geração: habilidades selecionadas,
  instrução pedagógica, duração e uso de recursos digitais.
- **Plano**: Rascunho privado associado a um professor, com Markdown, estado RASCUNHO e indicador de
  auxílio por IA.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Um professor com credenciais válidas consegue entrar e chegar à lista de rascunhos em até
  1 minuto, em testes de aceitação.
- **SC-002**: Um professor consegue localizar ao menos uma habilidade por cada critério de busca
  suportado e selecionar múltiplas habilidades em até 2 minutos, em testes de aceitação.
- **SC-003**: Com uma resposta válida, 100% das gerações aceitas nos testes criam exatamente um
  rascunho privado em estado RASCUNHO, marcado com auxílio por IA.
- **SC-004**: Em 100% dos cenários de falha de geração testados, nenhum plano parcial é listado e os
  dados fornecidos permanecem disponíveis para nova tentativa manual.
- **SC-004a**: Em testes sem resposta da IA, o estado de preparação termina em até 60 segundos com uma
  falha compreensível, sem criar rascunho.
- **SC-005**: Em 100% dos testes cruzados entre as duas contas de demonstração, um professor não consegue
  visualizar nem alterar o rascunho do outro.
- **SC-006**: Os fluxos de login, busca, geração, edição e lista permanecem concluíveis em verificações
  de desktop, tablet e celular.

## Assumptions

- As duas contas de demonstração usam credenciais conhecidas pela equipe de teste e representam dois
  professores distintos.
- O catálogo mínimo contém exemplos suficientes para exercitar todos os filtros exigidos, incluindo
  ao menos uma habilidade sem ano aplicável.
- Uma resposta válida da IA contém Markdown apto a ser apresentado e editado pelo professor.
- A lista inicial mostra rascunhos; estados finais, publicação, histórico de versões e exportação não
  fazem parte desta feature.
- O estado de preparação é visível enquanto houver uma solicitação de geração em andamento.
