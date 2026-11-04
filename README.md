# 100-days-of-golang

Repositório [Tech-Preta/100-days-of-golang](https://github.com/Tech-Preta/100-days-of-golang), branch padrão `main`.

A árvore versionada contém documentação de comunidade, configuração de revisão e workflows de GitHub Actions. Ela não contém arquivos `.go`, `go.mod`, `go.sum`, diretório de exercícios, `Makefile`, `Dockerfile` nem scripts de execução.

O texto anterior deste arquivo chamava o projeto de repositório de exemplo e pedia o botão **Use this template**. Nenhum arquivo versionado configura esse fluxo. **Pendente:** descrever, neste repositório, o propósito da trilha de 100 dias, o formato de cada dia e como o material deve ser executado.

## Propósito

O nome do repositório é `100-days-of-golang`. Os arquivos atuais não definem um programa de estudos, uma ordem de temas nem um critério de conclusão.

O que está implementado hoje:

- conduta, contribuição, segurança, changelog e licença
- modelo de issue para bug
- modelo de pull request, num caminho com espaço no nome (veja a estrutura)
- revisão automática do CodeRabbit
- hooks de pre-commit
- Dependabot para GitHub Actions
- lint e varreduras disparados pelo GitHub Actions

## Estrutura do repositório

```text
.
├── .coderabbit.yaml
├── .github/
│   ├── CODEOWNERS
│   ├── FUNDING.yml
│   ├── ISSUE_TEMPLATE/BUG_REPORT.md
│   ├── dependabot.yml
│   ├── workflows/
│   │   ├── checkov.yml
│   │   ├── dependency-review.yml
│   │   ├── issue.yml
│   │   ├── lint.yml
│   │   └── trivy.yml
│   └── (espaço)PULL_REQUEST_TEMPLATE/PULL_REQUEST_TEMPLATE.md
├── .gitignore
├── .pre-commit-config.yaml
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── SECURITY.md
```

O modelo de pull request está em `.github/ PULL_REQUEST_TEMPLATE/PULL_REQUEST_TEMPLATE.md`. O nome do diretório começa com um espaço. Não existe arquivo em `.github/PULL_REQUEST_TEMPLATE.md` nem diretório `.github/PULL_REQUEST_TEMPLATE/` sem esse espaço. **Pendente:** confirmar, fora deste Git, se o GitHub aplica esse modelo.

`.gitignore` existe e não lista padrões: o arquivo só tem uma quebra de linha. Nenhum caminho é ignorado por causa dele.

`CHANGELOG.md` declara [Mantenha um Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e [Versionamento Semântico](https://semver.org/lang/pt-BR/). A única versão registrada é `0.0.0`, datada de 2025-03-25, com o texto "Esse é apenas um exemplo de descrição." **Pendente:** substituir esse exemplo por mudanças reais quando houver release.

Workflows de OSV-Scanner e OpenSSF Scorecard não estão na árvore. O commit `af4999c` (`Remove OSV-Scanner and OpenSSF Scorecard workflows`) os removeu. Também não há `.github/workflows/dependabot.yml`; a configuração do Dependabot fica em `.github/dependabot.yml`.

## Pré-requisitos

Não há versão de Go declarada. Não há `go` directive, toolchain nem módulo.

O que os arquivos declaram para a automação:

| Peça            | Requisito escrito no repositório                                                        |
| --------------- | --------------------------------------------------------------------------------------- |
| Jobs de Actions | `runs-on: ubuntu-latest` nos cinco workflows versionados                                |
| Checkov         | Python `3.10`, via `actions/setup-python@v5`, e `pip install checkov` sem versão fixada |
| pre-commit      | `.pre-commit-config.yaml` fixa gitleaks `v8.16.3` e `pre-commit-hooks` `v4.4.0`         |

A ferramenta `pre-commit` em si não tem versão fixada no repositório, e não há comando de instalação versionado.

## Uso e execução

Não há programa para executar nem suíte de testes. `CONTRIBUTING.md` pede testes nas contribuições; a árvore não contém `*_test.go` nem outro arquivo de teste. **Pendente:** definir o comando de execução e de teste quando existir código.

O endereço deste repositório é `https://github.com/Tech-Preta/100-days-of-golang`, branch `main`. O guia em [`CONTRIBUTING.md`](CONTRIBUTING.md) ainda clona `https://github.com/Tech-Preta/repository_sample`. **Pendente:** alinhar essa URL ao nome real do repositório.

Os hooks declarados em [`.pre-commit-config.yaml`](.pre-commit-config.yaml) são `gitleaks`, `end-of-file-fixer` e `trailing-whitespace`. Não há script que os instale ou os rode.

O comportamento executável versionado está nos workflows abaixo. Eles rodam no GitHub Actions; este repositório não documenta um modo local equivalente para o Super-linter, o Dependency Review nem o upload de SARIF.

## Configuração

### CodeRabbit

[`.coderabbit.yaml`](.coderabbit.yaml) define `language: pt-BR`, perfil de review `chill` e `auto_review` habilitado. `drafts` está `false`. As ferramentas com `enabled: true` são shellcheck, ruff, markdownlint, github-checks, languagetool, biome, hadolint, swiftlint, phpstan, golangci-lint, yamllint, gitleaks, checkov, detekt, eslint, rubocop e buf.

Várias dessas ferramentas visam linguagens que não têm arquivos neste repositório. golangci-lint está habilitado no CodeRabbit; não há configuração `golangci` versionada nem código Go para ela analisar.

### Dependabot

[`.github/dependabot.yml`](.github/dependabot.yml) usa a versão 2 da configuração, ecossistema `github-actions`, diretório `/` e intervalo `daily`. Não há ecossistema `gomod`.

### Proprietários e financiamento

[`.github/CODEOWNERS`](.github/CODEOWNERS) atribui `*` a `@nataliagranato`.

[`.github/FUNDING.yml`](.github/FUNDING.yml) declara `github`, `ko_fi` e `buy_me_a_coffee` para `nataliagranato`, e `pix` com uma chave UUID. O valor da chave está só nesse arquivo.

### Segredos referenciados pelos workflows

| Segredo                | Onde aparece                                                                                                                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `secrets.GITHUB_TOKEN` | [`.github/workflows/lint.yml`](.github/workflows/lint.yml), no ambiente do Super-linter                                                                                                     |
| `secrets.USER_TOKEN`   | [`.github/workflows/trivy.yml`](.github/workflows/trivy.yml), campo `github-pat`; [`.github/workflows/issue.yml`](.github/workflows/issue.yml), variável `GITHUB_TOKEN` do step de resposta |

**Pendente:** o repositório não documenta a criação de `USER_TOKEN` nem os escopos exigidos.

## Integração contínua

Cinco workflows estão em [`.github/workflows/`](.github/workflows).

| Workflow                     | Arquivo                 | Disparo escrito no arquivo                                                                              |
| ---------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------- |
| Lint                         | `lint.yml`              | `push: null` e `pull_request: null`, sem filtro de branch                                               |
| Dependency Review            | `dependency-review.yml` | `pull_request`                                                                                          |
| Checkov Security Scan        | `checkov.yml`           | `push` em `main`, tags `v*`, `pull_request` (`opened`, `synchronize`, `reopened`) e `workflow_dispatch` |
| Generate SBOM                | `trivy.yml`             | `push` em `main`                                                                                        |
| Resposta Automática a Issues | `issue.yml`             | `issues` do tipo `opened`                                                                               |

Lint, Dependency Review e a resposta a issues usam `step-security/harden-runner` no commit `0634a2670c59f64b4a01f0f96f84700a4088b9f0` (comentário no arquivo: v2.12.0), com `egress-policy: audit`. Checkov e Trivy não usam esse step.

Uma consulta à API de Actions do repositório, fora do Git, também lista dois workflows dinâmicos como ativos: `Dependabot Updates` (`dynamic/dependabot/dependabot-updates`) e `CodeQL` (`dynamic/github-code-scanning/codeql`). As definições deles não estão na árvore.

[`SECURITY.md`](SECURITY.md) cita CodeQL e GitHub Advanced Security. Não há arquivo de workflow CodeQL em `.github/workflows/`. **Pendente:** registrar no Git onde o CodeQL e o GitHub Advanced Security estão configurados, se essa for a política desejada.

### Lint

Arquivo [`.github/workflows/lint.yml`](.github/workflows/lint.yml). Permissão do workflow: `{}`. Permissões do job: `contents: read`, `packages: read`, `statuses: write`.

O checkout é `actions/checkout` no commit `11bd71901bbe5b1630ceea73d27597364c9af683` (comentário: v4.2.2), com `fetch-depth: 0`. O comentário do arquivo diz que o Super-linter precisa do histórico completo para listar arquivos alterados entre commits.

O Super-linter é `super-linter/super-linter` no commit `4e8a7c2bf106c4c766c816b35ec612638dc9b6b2` (comentário: v7.3.0). A única variável de ambiente definida é `GITHUB_TOKEN`. O arquivo não define `VALIDATE_*` nem filtros de linguagem.

### Dependency Review

Arquivo [`.github/workflows/dependency-review.yml`](.github/workflows/dependency-review.yml). Permissão: `contents: read`. O checkout usa o mesmo commit do workflow de lint.

A action é `actions/dependency-review-action` no commit `ce3cf9537a52e8119d91fd484ab5b8a807627bf8` (comentário: v4.6.0).

O comentário no topo do arquivo descreve a função: analisar manifestos de dependência alterados no pull request e expor versões com vulnerabilidade conhecida. O arquivo diz ainda que, se o workflow for obrigatório, pull requests que introduzam pacotes vulneráveis conhecidos ficam impedidos de merge. Este repositório não versiona a regra que tornaria o check obrigatório.

### Checkov

Arquivo [`.github/workflows/checkov.yml`](.github/workflows/checkov.yml). Permissões: `contents: read` e `security-events: write` (o comentário do arquivo associa essa permissão ao upload de SARIF).

Passos escritos no arquivo:

1. `actions/checkout@v4`
2. `actions/setup-python@v5` com `python-version: "3.10"`
3. `pip install checkov`
4. `checkov -d ./ -o sarif --output-file checkov-results.sarif --quiet`
5. `github/codeql-action/upload-sarif@v3` com `sarif_file: checkov-results.sarif`
6. `checkov -d ./`

`actions/checkout`, `actions/setup-python`, `github/codeql-action/upload-sarif` e o pacote `checkov` estão por tag ou sem pin de versão. Os outros workflows citados acima fixam parte das actions por commit.

### SBOM com Trivy

Arquivo [`.github/workflows/trivy.yml`](.github/workflows/trivy.yml). Permissão: `contents: write`. Checkout: `actions/checkout@v4`.

A action é `aquasecurity/trivy-action` no commit `ed142fd0673e97e23eac54620cfb913e5ce36c25` (comentário: v0.36.0), com:

- `scan-type: fs`
- `format: github`
- `output: dependency-results.sbom.json`
- `image-ref: .`
- `github-pat: ${{ secrets.USER_TOKEN }}`

### Resposta automática a issues

Arquivo [`.github/workflows/issue.yml`](.github/workflows/issue.yml). Permissões: `contents: read` e `issues: write`.

O step usa `derekprior/add-autoresponse` no commit `500f8788d667c31d43a4469f8a3ec3b491470fd1`. O comentário no arquivo marca esse pin como `master`.

O step publica o texto configurado em `response`: um agradecimento a quem abriu a issue e o convite para escrever, em caso de bug, pergunta sobre o produto ou solicitação de funcionalidade, para `contato@nataliagranato.xyz`. O token passado em `GITHUB_TOKEN` é `secrets.USER_TOKEN`.

## Como contribuir

O fluxo escrito em [`CONTRIBUTING.md`](CONTRIBUTING.md) é: abrir issue com detalhes e passos de reprodução, quando couber; no pull request, fazer fork, clonar, criar branch, alterar o código, adicionar testes, commit, push e abrir o pull request contra `main`.

Leia também:

- [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md), adaptado do Contributor Covenant 2.1. Relatos de conduta vão para `contato@nataliagranato.xyz`.
- o modelo de issue [`.github/ISSUE_TEMPLATE/BUG_REPORT.md`](.github/ISSUE_TEMPLATE/BUG_REPORT.md). O front matter pede o título `[BUG]`, a label `bug` e o assignee `nataliagranato`.

  O corpo pergunta método de execução (Kubernetes com manifestos, Kubernetes com Helm ou Docker local), sistema operacional, navegador e versão da aplicação. Não há manifestos Kubernetes, chart Helm, Dockerfile nem aplicação com versão executável na árvore.

  **Pendente:** ajustar esse modelo ao que o repositório de fato contém, ou adicionar esses artefatos se forem parte do projeto.

- o modelo de pull request no caminho com espaço descrito na estrutura. O checklist pede testes e cita Kubernetes, Helm, Docker e GitHub Actions. Só GitHub Actions tem arquivos correspondentes hoje.

`SECURITY.md` afirma code review obrigatório para todas as alterações. O Git deste repositório não contém regra de proteção de branch. **Pendente:** a exigência de review, se for o caso, está fora dos arquivos versionados.

## Segurança

[`SECURITY.md`](SECURITY.md) marca a versão `0.0.0` como suportada. O reporte de vulnerabilidade pede para não abrir issue pública e enviar email para `contato@nataliagranato.xyz`, com descrição, passos para reproduzir, versão afetada e impacto possível. O próprio arquivo descreve os prazos esperados: confirmação em até 48 horas e avaliação inicial em até 7 dias.

O mesmo arquivo lista estas práticas e ferramentas: code review obrigatório, Dependabot, CodeQL, GitHub Advanced Security, Trivy e Checkov. Na árvore, Dependabot, Trivy e Checkov têm arquivo de configuração ou workflow. CodeQL, GitHub Advanced Security e a obrigatoriedade de review não têm arquivo correspondente; veja a seção de integração contínua.

## Licença

[`LICENSE`](LICENSE) é o texto integral da GNU General Public License, versão 3, de 29 de junho de 2007. O copyright desse texto é da Free Software Foundation. O repositório não acrescenta outro aviso de copyright do projeto.

## Contato

- Código: `@nataliagranato`, em [`.github/CODEOWNERS`](.github/CODEOWNERS).
- Segurança e conduta: `contato@nataliagranato.xyz`, em [`SECURITY.md`](SECURITY.md) e [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).
- Financiamento: [`.github/FUNDING.yml`](.github/FUNDING.yml).

## Pendências

Itens ausentes ou não confirmados pelos arquivos versionados:

1. Propósito da trilha, índice de dias, formato dos exercícios e comando para executar ou testar.
2. Versão do Go e módulo (`go.mod` / `go.sum`).
3. URL de clone em `CONTRIBUTING.md`, que ainda usa `Tech-Preta/repository_sample`.
4. Efeito do diretório de pull request cujo nome começa com espaço.
5. Criação e escopos do segredo `USER_TOKEN`.
6. Texto real do changelog no lugar do exemplo da versão `0.0.0`.
7. Padrões em `.gitignore`, quando houver artefatos gerados para ignorar.
8. Campos de Kubernetes, Helm, Docker, navegador e versão da aplicação nos modelos de issue e pull request.
9. Onde ficam, se existirem fora do Git, a obrigatoriedade de code review, o CodeQL e o GitHub Advanced Security citados em `SECURITY.md`.
