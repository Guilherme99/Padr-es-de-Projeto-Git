# Padrões de Projeto Git
Este documento define os padrões utilizados neste repositório para:
- **Estrutura de branches**
- **Nomes de branchs**
- **Commits**
- **Merge requests**

## Estrutura de Branches Padrão

Este repositório utiliza três branches principais que devem sempre existir:

### Branches Principais:

| Branch       | Descrição                                                   | Uso                                           |
|-------------|-------------------------------------------------------------|-----------------------------------------------|
| `production` | Branch principal contendo código estável em produção       | Deploy automático para ambiente de produção  |
| `staging`    | Branch para testes pré-produção e validação final          | Deploy automático para ambiente de staging   |
| `develop`    | Branch de integração para desenvolvimento contínuo         | Ambiente de desenvolvimento e testes internos|

### Fluxo de Trabalho:
```
feature branches → develop → staging → production
```

**Regras importantes:**
- **Nunca** faça commit diretamente nas branches principais
- Todas as mudanças devem passar por **Pull/Merge Requests**
- Somente código aprovado e testado deve ser mergeado

---

## Padrão de Nomes de Branch

Use o seguinte formato para criar branches de trabalho:

### Tipos comuns permitidos:

| Tipo      | Descrição                           | Exemplo sem Ferramento Ágil      | Exemplo c/ Ferramento Ágil |
|-----------|-------------------------------------|----------------------------------|----------------------------|
| `feature` | Nova funcionalidade                 | `feature/login-page`             | `feature/SG-123`           |
| `bugfix`  | Correção de bug                     | `bugfix/fix-auth-token`          | `bugfix/SG-123`            |
| `chore`   | Tarefas de manutenção               | `chore/update-dependencies`      | `chore/SG-123`             |
| `refactor`| Refatoração sem mudar comportamento | `refactor/clean-auth-module`     | `refactor/SG-123`          |
| `test`    | Adição ou alteração de testes       | `test/add-login-tests`           | `test/SG-123`              |
| `docs`    | Adição de documentação              | `docs/add-docs`                  | `docs/SG-123`              |
| `hotfix`  | Correção urgente em produção        | `hotfix/critical-security-fix`   | `hotfix/SG-456`            |

### Criação de Branches:
```bash
# Branches de feature/bugfix (a partir do develop)
git checkout develop
git pull origin develop
git checkout -b feature/nova-funcionalidade

# Branches de hotfix (a partir do production)
git checkout production
git pull origin production
git checkout -b hotfix/correcao-critica
```

---

## ✍️ Padrão de Commits

Adotamos o padrão: `<tipo>(escopo): <descrição>`
Link: **[Conventional Commits](https://www.conventionalcommits.org/)**:

### Tipos comuns:

**feat** - Commits do tipo feat indicam que seu trecho de código está incluindo um novo recurso (se relaciona com o MINOR do versionamento semântico).

**fix** - Commits do tipo fix indicam que seu trecho de código commitado está solucionando um problema (bug fix), (se relaciona com o PATCH do versionamento semântico).

**docs** - Commits do tipo docs indicam que houveram mudanças na documentação, como por exemplo no Readme do seu repositório. (Não inclui alterações em código).

**test** - Commits do tipo test são utilizados quando são realizadas alterações em testes, seja criando, alterando ou excluindo testes unitários. (Não inclui alterações em código)

**build** - Commits do tipo build são utilizados quando são realizadas modificações em arquivos de build e dependências.

**perf** - Commits do tipo perf servem para identificar quaisquer alterações de código que estejam relacionadas a performance.

**style** - Commits do tipo style indicam que houveram alterações referentes a formatações de código, semicolons, trailing spaces, lint... (Não inclui alterações em código).

**refactor** - Commits do tipo refactor referem-se a mudanças devido a refatorações que não alterem sua funcionalidade, como por exemplo, uma alteração no formato como é processada determinada parte da tela, mas que manteve a mesma funcionalidade, ou melhorias de performance devido a um code review.

**chore** - Commits do tipo chore indicam atualizações de tarefas de build, configurações de administrador, pacotes... como por exemplo adicionar um pacote no gitignore. (Não inclui alterações em código)

**ci** - Commits do tipo ci indicam mudanças relacionadas a integração contínua (continuous integration).

**raw** - Commits do tipo raw indicam mudanças relacionadas a arquivos de configurações, dados, features, parâmetros.

**cleanup** - Commits do tipo cleanup são utilizados para remover código comentado, trechos desnecessários ou qualquer outra forma de limpeza do código-fonte, visando aprimorar sua legibilidade e manutenibilidade.

**remove** - Commits do tipo remove indicam a exclusão de arquivos, diretórios ou funcionalidades obsoletas ou não utilizadas, reduzindo o tamanho e a complexidade do projeto e mantendo-o mais organizado.

### Tabela resumida: 
| Tipo       | Uso                                                       | Exemplo                                           |
| ---------- | --------------------------------------------------------- | ------------------------------------------------- |
| `feat`     | Nova funcionalidade                                       | `feat(auth): add JWT login endpoint`              |
| `fix`      | Correção de bug                                           | `fix(auth): handle null token`                    |
| `docs`     | Alterações na documentação                                | `docs(readme): update API usage section`          |
| `test`     | Criação/alteração de testes (unitários, integração, etc.) | `test(login): add unit tests for login`           |
| `build`    | Alterações em arquivos de build ou dependências           | `build(project): update webpack config`           |
| `perf`     | Melhorias de performance                                  | `perf(api): optimize database query`              |
| `style`    | Formatação, lint, espaços, etc. (sem mudança de lógica)   | `style(lint): fix eslint trailing spaces`         |
| `refactor` | Refatorações sem alterar comportamento                    | `refactor(auth): simplify token validation logic` |
| `chore`    | Tarefas administrativas/configurações gerais              | `chore(deps): update dependencies`                |
| `ci`       | Alterações em pipelines de integração contínua            | `ci(github): add GitHub Actions workflow`         |
| `raw`      | Alterações em arquivos de configuração, dados, parâmetros | `raw(data): update seed data for testing`         |
| `cleanup`  | Limpeza de código (comentários, trechos obsoletos)        | `cleanup(code): remove commented-out logic`       |
| `remove`   | Exclusão de arquivos, diretórios ou funcionalidades       | `remove(auth): delete unused auth service`        |

---

## Padrão de Merge Request (MR)

### Título
`[TIPO-BRANCH] Descrição breve da alteração`

Exemplos:
- `[FEATURE] Implementação do sistema de login`
- `[BUGFIX] Correção no processo de autenticação`
- `[HOTFIX] Correção crítica de segurança`

### Corpo da Descrição
```markdown
## O que foi alterado/criado?
[Descrição detalhada das mudanças]

## Por que essa mudança é necessária?
[Contexto e justificativa]

## Como testar?
[Passos para validar as alterações]

## Screenshots (se aplicável)
[Imagens demonstrando as mudanças visuais]

## Checklist
- [ ] Código revisado e testado localmente
- [ ] Testes unitários passando
- [ ] Documentação atualizada (se necessário)
- [ ] Pipeline de CI passou
```

### Fluxo de Aprovação por Branch:

| Branch Destino | Revisores Necessários | Testes Obrigatórios |
|---------------|-----------------------|---------------------|
| `develop`     | 1 revisor             | Testes unitários    |
| `staging`     | 2 revisores           | Testes de integração|
| `production`  | 2+ revisores + Lead   | Todos os testes     |

### Sobre MR
Ao visualizar uma solicitação de mesclagem, você verá:

- Uma descrição do pedido
- Alterações de código e revisões de código em linha
- Informações sobre pipelines de CI/CD
- Relatórios de capacidade de fusão
- Comentários
- A lista de commits

#### Notas Importantes
- **Sempre** se autoassociar ao MR criado
- **Sempre** selecionar um Revisor apropriado para aprovação
- **Nunca** fazer merge sem aprovação
- **Aguardar** todos os checks de CI/CD passarem antes do merge
- Para **hotfixes**: notificar o time imediatamente sobre o merge em produção

---

## Tags e Versionamento

### Onde Criar Tags:
| `production` | Versões estáveis      | `v1.0.0`, `v1.1.0`       |

### Comando para Criar Tags:
```bash
# Em produção (versão final)
git checkout production
git tag -a v1.0.0 -m "Release 1.0.0 - Nova funcionalidade X"
git push origin v1.0.0
```

## Fontes
- https://git-scm.com/book/pt-br/v2/Branches-no-Git-O-b%C3%A1sico-de-Ramifica%C3%A7%C3%A3o-Branch-e-Mesclagem-Merge
- https://www.conventionalcommits.org/en/v1.0.0/
- https://github.com/iuricode/padroes-de-commits
