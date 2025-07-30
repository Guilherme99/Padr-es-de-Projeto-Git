# Padrões de Projeto Git
Este documento define os padrões utilizados neste repositório para:
- **Estrutura de branches**
- **Nomes de branchs**
- **Commits**
- **Tags e Versionamento**

## Estrutura de Branches Padrão

Este repositório utiliza a seguinte estrutura hierárquica de branches:

### Estrutura Visual:
```
main (production)
├── hotfix/critical-fix → main
├── release/v1.0.0 → main
│   └── develop
│       ├── feature branchs
```

### Branches Principais:

| Branch    | Descrição                                                   | Uso                                           |
|-----------|-------------------------------------------------------------|-----------------------------------------------|
| `main`    | Branch principal contendo código estável em produção        | Deploy automático para ambiente de produção   |
| `develop` | Branch de integração para desenvolvimento contínuo          | Ambiente de desenvolvimento e testes internos |
| `release` | Branch para preparação de versões e testes pré-produção     | Estabilização e validação final               |
| `hotfix`  | Branch para correções críticas direto em produção           | Correções emergenciais                        |

### Fluxo de Trabalho:
```
feature branches → develop → release → main
hotfix branches → main (direto)
```
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
| `release` | Preparação de versão                | `release/v1.2.0`                 | `release/v1.2.0`           |

### Criação de Branches:
```bash
# Branches de feature/bugfix (a partir do develop)
git checkout develop
git pull origin develop
git checkout -b feature/nova-funcionalidade

# Branches de release (a partir do develop)
git checkout develop
git pull origin develop
git checkout -b release/v1.2.0

# Branches de hotfix (a partir do main)
git checkout main
git pull origin main
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

### Estratégias de Merge por Tipo de Branch:

| Tipo de Branch              | Estratégia de Merge | Justificativa                                      |
|-----------------------------|---------------------|----------------------------------------------------|
| `feature/*` → `develop`     | **Squash commits**  | Histórico mais limpo, um commit por funcionalidade |
| `bugfix/*` → `develop`      | **Squash commits**  | Histórico mais limpo, um commit por correção       |
| `release/*` → `main`        | **Merge commits**   | Preservar contexto completo da release             |
| `develop` → `release/*`     | **Merge commits**   | Preservar contexto das features incluídas          |
| `hotfix/*` → `main`         | **Fast-forward**    | Para correções simples e diretas                   |

# Squash merge
git merge --squash feature/branch-name
git commit -m "type(escopo): description"

# Merge commit (preservar histórico)
git merge --no-ff branch-name

# Fast-forward (linear)
git merge --ff-only branch-name

---

## Tags e Versionamento

### Versionamento Semântico (SemVer)

Seguimos o padrão **MAJOR.MINOR.PATCH** conforme [Semantic Versioning](https://semver.org/):

| Componente | Quando Incrementar                  | Exemplos de Mudanças                                            |
|------------|-------------------------------------|-----------------------------------------------------------------|
| **MAJOR**  | Mudanças incompatíveis na API       | Remoção de endpoints, mudança de contratos, breaking changes    |
| **MINOR**  | Novas funcionalidades (compatíveis) | Novos endpoints, novas features, melhorias sem breaking changes |
| **PATCH**  | Correções de bugs (compatíveis)     | Hotfixes, correções de segurança, pequenos ajustes              |

### Critérios Específicos por Tipo:

#### MAJOR (X.0.0) - Breaking Changes
- Remoção ou mudança de APIs públicas
- Alteração de schemas de banco incompatíveis
- Mudança de comportamento esperado pelos usuários
- Remoção de funcionalidades existentes
- Mudança de versões principais de dependências (Ex. Node 16→18, Java 8 -> 11)

#### MINOR (X.Y.0) - New Features
- Adição de novas funcionalidades
- Novos endpoints de API
- Melhorias de performance
- Novas opções de configuração (com defaults)
- Deprecação de funcionalidades (com aviso)

#### PATCH (X.Y.Z) - Bug Fixes
- Correções de bugs
- Hotfixes de segurança
- Ajustes de documentação
- Correções de typos

### Onde Criar Tags:

| Branch      | Tipo de Versão   | Exemplos                                         |
|-------------|------------------|--------------------------------------------------|
| `main`      | Versões estáveis | `v1.0.0`, `v1.2.3`, `v2.0.0`                     |

### Comandos para Criar Tags:

```bash
# Versão estável (main)
git checkout main
git tag -a v1.2.0 -m "Release 1.2.0 - Sistema de notificações"
git push origin v1.2.0

# Hotfix (main - patch)
git checkout main
git tag -a v1.1.1 -m "Hotfix 1.1.1 - Correção crítica de segurança"
git push origin v1.1.1
```

### Changelog
> - Gerar changelog automaticamente baseado nos commits convencionais

## Fontes
- https://git-scm.com/book/pt-br/v2/Branches-no-Git-O-b%C3%A1sico-de-Ramifica%C3%A7%C3%A3o-Branch-e-Mesclagem-Merge
- https://www.conventionalcommits.org/en/v1.0.0/
- https://github.com/iuricode/padroes-de-commits
- https://git-scm.com/book/en/v2/Git-Basics-Tagging
- https://semver.org/
