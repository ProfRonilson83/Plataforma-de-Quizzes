# Plataforma de Revisões — UniESBAM + UNINORTE

Sistema estático para GitHub Pages, com sete disciplinas e 140 questões originais de revisão (20 por disciplina, 0,5 ponto cada). Cada enunciado possui seis linhas organizadas em situação profissional, problema e comando. Ao finalizar, o aluno recebe os comentários das quatro alternativas. As questões se inspiram nos temas das provas fornecidas, mas não constituem cópia ou gabarito oficial.

## Pastas / arquivos
- `index.html` — interface dos alunos e professor.
- `app.js` — autenticação, sincronização e controle das revisões no Firebase.
- `questoes.json` — 7 conjuntos de 20 questões (editável para revisão pelo docente).
- `config.js` — credenciais públicas do projeto Firebase e UID/e-mail do professor; sem senhas.
- `firestore.rules` — regras de permissão do sistema multidisciplinar **para o novo projeto independente**.

## Configuração e implantação
1. No Firebase `plataforma-quiz-2026`, habilitar Authentication **Anônimo** e **E-mail/senha** e criar o professor em Authentication > Usuários.
2. Confirmar em `config.js` se `professorEmail` corresponde **exatamente** ao cadastro do professor. O UID configurado é `skgAGL0AquhmW9CIb6kJypYSi7s2`.
3. Abrir Cloud Firestore > `(default)` > Regras; fazer backup das regras atuais; substituir pelo conteúdo de `firestore.rules` e clicar em **Publicar**. Este projeto usa as coleções `assessments` e `assessments/{id}/attempts`. Não é necessário modificar as regras do Firebase antigo `quiz-uniesbam`.
4. Em Authentication > Configurações > Domínios autorizados, autorizar `profronilson83.github.io` caso necessário.
5. Criar um repositório GitHub **novo**, por exemplo `revisoes-7-disciplinas`, para não substituir a página `av2frontend` existente. Enviar os cinco arquivos na raiz do repositório. Settings > Pages > Deploy from branch > main > root.
6. Visitar `https://profronilson83.github.io/revisoes-7-disciplinas/`, clicar em **Painel do professor**, entrar com e-mail/senha Firebase. No painel clicar em **Inicializar os 7 quizzes no Firebase** (essa ação cria apenas documentos que não existem).
7. Selecionar a disciplina no painel e clicar **Abrir esta revisão**. Os alunos podem então entrar pelo mesmo link, escolher a disciplina e preencher nome, matrícula e turma.
8. Os registros começam a aparecer no painel da disciplina com quantidade respondida, nota parcial e status online. Os alunos precisam continuar com navegador ativo; aba suspensa ou conexão interrompida pode atrasar a atualização.
9. Para guardar as notas clique **Exportar CSV** antes de **Nova sessão**. A nova sessão exclui as participações da disciplina selecionada; não muda outras disciplinas.

### Links diretos por disciplina
Adicionar ao endereço da plataforma `?disciplina=arduino`, `?disciplina=banco`, `?disciplina=poo`, `?disciplina=frontend`, `?disciplina=so`, `?disciplina=backend`, `?disciplina=coding`.

### Segurança e limitações
- As regras permitem que cada aluno autenticado anonimamente leia o próprio registro e o professor consulte todos. Nomes e matrículas são dados pessoais; divulgue o propósito do tratamento, restrinja o acesso e defina período de retenção.
- **Uso indicado: estudo e revisão, não prova oficial com valor de nota.** Por ser aplicação estática, o gabarito e as respostas ficam disponíveis no arquivo JSON ou nos documentos de avaliação, e a nota parcial é calculada no navegador. Para avaliação formal, implementar gabarito privado e correção em ambiente de servidor (por exemplo, Cloud Functions) com validação de identidade.
- A pontuação é calculada por número de acertos × 0,5, e os resultados obtidos por dispositivos diferentes aparecem no mesmo Firestore, desde que todos usem o projeto Firebase e as regras sejam publicadas.
- Em caso de `Missing or insufficient permissions`, verificar se `firestore.rules` foi efetivamente publicado e se o professor tem UID correspondente no Authentication.
- Não publicar senhas, credenciais de serviço, chaves privadas ou arquivos `.env` no GitHub.
- O plano Spark tem limites gratuitos de operações; acompanhe o uso no Console.

### Antes de usar com os alunos
Revise as 140 questões e seus comentários em `questoes.json` quanto a clareza didática, dificuldade e adequação ao seu plano de ensino. As avaliações originais tinham questões discursivas; esta versão é exclusivamente objetiva, criada como revisão.

## Diagnóstico do erro de permissão (nova versão)
1. Entre com a conta de professor no site publicado. Clique em **Diagnosticar permissões**.
2. Confira se o UID autenticado corresponde a `3Rl7hf3QbwPfmdSWgZcbqg7J2Rj2`. Se não corresponder, a conta logada está errada: saia e entre com a conta cadastrada em Authentication > Usuários.
3. Se o UID corresponder, mas houver BLOQUEADO, abra o projeto `quiz-uniesbam`, banco `(default)`, aba **Firestore > Regras**. Faça backup e cole **todo** o conteúdo de `firestore.rules`, depois clique **Publicar**. O arquivo local no GitHub NÃO publica regras automaticamente.
4. Confirme que o Console exibe as regras publicadas (e não apenas alteradas no editor). Atualize o site com Ctrl+Shift+R e rode o diagnóstico novamente.
5. Se o UID estiver correto e as regras efetivamente publicadas, mas ainda houver bloqueio, confira o `projectId` do `config.js` e envie o relatório de diagnóstico com a rota BLOQUEADO (sem senha) para análise.

**Observação:** não altere as regras para `allow read, write: if true;`. Não ative modo de teste. Um erro de permissão pode ocorrer porque as regras não foram publicadas no banco certo, porque a conta autenticada não é a autorizada, ou porque a rota consultada não tem a permissão necessária.

## Inicialização no projeto novo
1. Confirme que o `config.js` publicado mostra `projectId: "plataforma-quiz-2026"`.
2. Publique o `firestore.rules` desta pasta no Firestore `(default)` **do mesmo projeto novo**.
3. Entre no site e no painel do professor usando `ronilson.silva@esbam.edu.br` e a senha cadastrada no **novo** Authentication.
4. Clique em **Diagnosticar permissões** e confira `UID corresponde: SIM` e leituras `PERMITIDO`.
5. Clique em **Inicializar os 7 quizzes no Firebase**. Isso cria os documentos `assessments/{id}` que ainda não existem, inicialmente fechados; não apaga documentos existentes.
6. Clique em **Abrir esta revisão** separadamente para cada disciplina.
7. Teste em outro aparelho como aluno.

**Nota:** os quizzes são material de revisão. O gabarito fica acessível no navegador, portanto não é adequado usar como avaliação oficial sem correção protegida no servidor.


## Prazo de estudo: 7 dias por disciplina

- Na primeira vez que o professor clica **Abrir esta revisão**, o sistema grava `openedAt` e `expiresAt` (168 horas depois) no documento `assessments/<disciplina>`.
- O horário de término aparece no painel do professor. Cada disciplina possui seu próprio prazo.
- O bloqueio é verificado no navegador e, principalmente, nas regras do Firestore por `request.time.toMillis()`, inclusive se nenhum computador do professor estiver ligado.
- **Fechar e reabrir** a mesma sessão não prolonga o prazo. Depois da expiração, novas respostas são recusadas.
- Para abrir um novo ciclo de sete dias, **exporte as notas** antes de usar **Nova sessão desta disciplina**; esta opção apaga as tentativas anteriores.
- Atualize no GitHub `index.html`, `app.js` e publique no Firebase o novo `firestore.rules`.
- Se a disciplina já estava aberta antes da atualização e não tinha `expiresAt`, clique Fechar e depois Abrir para iniciar a contagem. Enquanto isso, o acesso dos alunos ficará bloqueado com as regras novas.
- Este sistema usa o relógio do professor para definir o instante da abertura; o Firestore verifica no servidor o instante de expiração das gravações.
