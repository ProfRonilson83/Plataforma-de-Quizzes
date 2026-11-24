# Plataforma de Revisões — UniESBAM + UNINORTE

Sistema estático para GitHub Pages, com sete disciplinas e 140 questões originais de revisão (20 por disciplina, 0,5 ponto cada). Cada enunciado possui seis linhas organizadas em situação profissional, problema e comando. Ao finalizar, o aluno recebe os comentários das quatro alternativas. As questões se inspiram nos temas das provas fornecidas, mas não constituem cópia ou gabarito oficial.

## Pastas / arquivos
- `index.html` — interface dos alunos e professor.
- `app.js` — autenticação, sincronização e controle das revisões no Firebase.
- `questoes.json` — 7 conjuntos de 20 questões (editável para revisão pelo docente).
- `config.js` — credenciais públicas do projeto Firebase e UID/e-mail do professor; sem senhas.
- `firestore.rules` — regras de permissão do sistema multidisciplinar **mais compatibilidade com o quiz antigo**.

## Configuração e implantação
1. No Firebase `quiz-uniesbam`, habilitar Authentication **Anônimo** e **E-mail/senha** e criar o professor em Authentication > Usuários.
2. Confirmar em `config.js` se `professorEmail` corresponde **exatamente** ao cadastro do professor. O UID configurado é `3Rl7hf3QbwPfmdSWgZcbqg7J2Rj2`.
3. Abrir Cloud Firestore > `(default)` > Regras; fazer backup das regras atuais; substituir pelo conteúdo de `firestore.rules` e clicar em **Publicar**. Essa versão inclui as rotas `quiz/settings`, `attempts` do sistema anterior.
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
