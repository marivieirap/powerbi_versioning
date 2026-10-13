# Versionamento Git para Projetos Power BI

**Autora:** Mariana Vieira Paiva  

## 1. Visão geral

Este projeto demonstra como utilizar Git e GitHub para controlar versões de um projeto desenvolvido no Power BI, usando um calendário como exemplo prático.

A proposta é acompanhar a evolução do projeto por meio de commits pequenos e descritivos, entender quais arquivos foram alterados e demonstrar como recuperar o conteúdo de uma versão anterior sem apagar o histórico mais recente.

O projeto utiliza o formato **Power BI Project (`.pbip`)**, que mantém as definições do relatório e do modelo semântico organizadas em uma estrutura de pastas. Essa organização permite que o Git acompanhe as alterações com mais granularidade do que em um arquivo `.pbix` tradicional.

### Objetivos

- Entender os conceitos básicos de Git aplicados a um projeto Power BI.
- Registrar mudanças incrementais com commits identificáveis.
- Comparar versões e localizar os arquivos alterados.
- Restaurar o conteúdo de uma versão anterior e registrar essa restauração como um novo commit.
- Explorar os benefícios e as limitações do versionamento de projetos Power BI.

## 2. Pré-requisitos

Para reproduzir o experimento, você precisará de:

- **Power BI Desktop**, com suporte ao formato Power BI Project (`.pbip`).
- **Git** instalado na máquina.
- Uma conta no **GitHub** e um repositório para armazenar o projeto.
- Conhecimentos básicos de terminal e dos comandos `status`, `add`, `commit`, `log`, `diff` e `restore`.

Consulte também a documentação oficial da Microsoft sobre [Power BI projects (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview).

## 3. Estrutura do projeto

A estrutura principal do repositório é:

```text
powerbi_versioning/
├── Calendario.Report/
│   └── definition.pbir
├── Calendario.SemanticModel/
├── .gitignore
├── Calendario.pbip
└── README.md
```

Os diretórios do relatório e do modelo semântico contêm as definições do projeto. O arquivo `Calendario.pbip`, na raiz, funciona como ponto de entrada para abrir o projeto no Power BI Desktop.

O arquivo `.gitignore` evita o versionamento de arquivos locais e de cache que não são necessários para compartilhar o projeto, incluindo arquivos `.pbix`:

```gitignore
**/.pbi/localSettings.json
**/.pbi/cache.abf
*.pbix
```

O arquivo `.pbix` foi removido do repositório no commit `rev006`, pois não é necessário para demonstrar o versionamento do projeto em formato PBIP.

> **Importante:** mantenha a estrutura completa de pastas e arquivos do projeto. O arquivo `Calendario.pbip` referencia as definições que ficam nos diretórios do relatório e do modelo semântico.

## 4. Configuração inicial e fluxo de trabalho Git

Depois de instalar o Git e clonar o repositório, o fluxo básico para registrar uma alteração é:

```bash
git status
git add .
git commit -m "Descrição objetiva da alteração"
git push
```

O que cada comando faz:

- `git status`: mostra os arquivos modificados, adicionados ou removidos.
- `git add .`: prepara as alterações do diretório atual para o próximo commit. Revise o resultado antes de confirmar.
- `git commit -m "..."`: registra um ponto no histórico local.
- `git push`: envia os commits locais para o repositório remoto.

Para consultar o histórico de forma resumida:

```bash
git log --oneline
```

Para analisar uma alteração antes de criar o commit:

```bash
git diff
```

É recomendável fazer commits pequenos, cada um relacionado a uma mudança lógica. Isso facilita a revisão, a comparação entre versões e a identificação de quando determinada alteração foi introduzida.

## 5. Organização dos commits

Os commits deste experimento foram nomeados sequencialmente, de `rev000` a `rev011`, para facilitar a leitura da evolução do projeto.

| Commit | Descrição |
|---|---|
| `rev000` | Commit inicial: modelo base do calendário |
| `rev001` | Criação do README |
| `rev002` | Atualização da anotação `PBI_ProTooling` do modelo |
| `rev003` | Formato curto na coluna Data, ordenação de Mês por MesNum e criação de Dia e Nome do dia |
| `rev004` | Adição da tabela de medidas |
| `rev005` | Adição das tabelas `tb_Calendar` e `tb_Feriados` e dos visuais do relatório |
| `rev006` | Remoção do PBIX e atualização do `.gitignore` |
| `rev007` | Ajuste da medida `HTML_Calendario` e inclusão de filtros de Ano e Mês |
| `rev008` | Criação do modo escuro |
| `rev009` | Definição do modo escuro como padrão |
| `rev010` | Definição do modo claro como padrão |
| `rev011` | Restauração do projeto para o estado da `rev009` |

A sequência mostra como mudanças no modelo, no relatório e na configuração visual podem ser registradas em etapas independentes. Os nomes `rev000` a `rev011` são rótulos adotados para este estudo, não uma exigência do Git.

## 6. Como visualizar as alterações entre versões

O comando `git log --oneline` ajuda a identificar os commits. Para ver quais arquivos foram modificados em um commit específico:

```bash
git show --stat <hash-do-commit>
```

Para comparar os arquivos do relatório entre duas versões, use:

```bash
git diff <hash-anterior> <hash-posterior> -- Calendario.Report/
```

Para comparar as definições do modelo semântico:

```bash
git diff <hash-anterior> <hash-posterior> -- Calendario.SemanticModel/
```

Substitua os placeholders pelos hashes reais dos commits. Por exemplo, para investigar a alteração visual introduzida em `rev008`, compare o commit anterior com o hash de `rev008`, direcionando o comando para `Calendario.Report/`.

O Git consegue mostrar diferenças em arquivos de texto que representam as definições do projeto. No entanto, nem toda alteração feita no Power BI será apresentada como uma diferença fácil de interpretar: a legibilidade depende do arquivo e de como a informação foi serializada.

## 7. Experimento prático: restaurando uma versão anterior

O objetivo deste experimento foi recuperar o estado do projeto correspondente à `rev009`, quando o modo escuro estava definido como padrão, mesmo depois de a `rev010` ter alterado o padrão para o modo claro.

O comando utilizado foi:

```bash
git restore --source=9af6adb --staged --worktree .
```

Nesse comando:

- `--source=9af6adb` indica o commit cujos conteúdos serão usados como referência.
- `--staged` atualiza a área de staging, também conhecida como índice.
- `--worktree` atualiza os arquivos do diretório de trabalho.
- `.` aplica a operação aos caminhos abrangidos pelo diretório atual.

**Atenção:** esse comando substitui as versões atuais dos arquivos rastreados pelos conteúdos da versão de origem, tanto na área de staging quanto no diretório de trabalho. Alterações locais nesses arquivos podem ser sobrescritas. Antes de executar, confira `git status` e, se necessário, faça backup ou commit das mudanças que deseja preservar.

O comando não remove arquivos não rastreados. Portanto, arquivos criados localmente que não estejam sob controle do Git não são apagados por essa operação.

Depois de validar o relatório no Power BI Desktop, a restauração foi registrada como um novo commit:

```bash
git add .
git commit -m "rev011 Restaura o projeto para o estado da rev009"
git push
```

O resultado é que os arquivos rastreados passam a refletir o estado da `rev009`, enquanto o histórico continua preservando os commits anteriores, incluindo a `rev010`, e registra a ação de restauração na `rev011`.

## 8. Por que criar um novo commit?

Em vez de reescrever o histórico, o experimento registra a restauração como um novo commit. Isso torna a intenção explícita e permite acompanhar quando a restauração do estado dos arquivos foi realizada.

Há mais de uma maneira de desfazer mudanças com Git, e a escolha depende do objetivo:

| Comando | Comportamento | Quando considerar |
|---|---|---|
| `git restore` | Restaura conteúdo de arquivos a partir de uma origem, atualizando a área de staging e/ou o diretório de trabalho conforme as opções usadas. | Para recuperar o conteúdo de arquivos de uma versão específica. |
| `git revert` | Cria um novo commit que desfaz as alterações introduzidas por outro commit. | Para desfazer um commit sem removê-lo do histórico, especialmente em branches compartilhadas. |
| `git reset` | Move o ponteiro da branch para outro commit e pode alterar o staging e o diretório de trabalho, conforme o modo usado. | Para reorganizar o histórico local com cuidado; requer atenção em branches compartilhadas. |

Neste caso, `git revert` também poderia ser uma opção se o objetivo fosse desfazer as mudanças introduzidas pela `rev010`. A abordagem com `restore` foi escolhida para demonstrar como recuperar os conteúdos rastreados de um commit específico e registrar essa restauração em um novo commit.

A operação usada não apagou a `rev010` do histórico nem moveu a branch para trás. Em vez disso, criou a `rev011`, documentando a restauração.

## 9. Como abrir o projeto

Para abrir o relatório no Power BI Desktop:

1. Clone ou baixe este repositório.
2. Extraia os arquivos, caso tenha baixado o arquivo ZIP.
3. Navegue até a pasta `Calendario.Report/`.
4. Abra o arquivo `definition.pbir` no Power BI Desktop para acessar o relatório com a versão mais recente dos visuais.
5. Explore as páginas do relatório, os filtros e as interações.

> **Observação:** mantenha a estrutura de pastas do repositório para que o relatório consiga referenciar corretamente o modelo semântico associado.

O GitHub permite visualizar os arquivos versionados, mas não executa nem renderiza o relatório Power BI interativo diretamente na página do repositório.

## 10. Aprendizados, limitações e conclusão

Este experimento demonstrou que o formato PBIP permite acompanhar no Git as definições do relatório e do modelo semântico, facilitando a comparação entre versões e a documentação das mudanças.

Principais aprendizados:

- Commits pequenos e descritivos tornam o histórico mais compreensível.
- O `git diff` ajuda a identificar quais arquivos mudaram entre versões.
- É possível restaurar o conteúdo de uma versão anterior sem remover os commits posteriores do histórico.
- Uma restauração deve ser validada no Power BI Desktop, porque a comparação textual não substitui a verificação do comportamento visual e funcional do relatório.
- Arquivos locais, cache e dados que não precisam ser compartilhados devem ser avaliados para inclusão no `.gitignore`.

O versionamento também tem limitações. O Git não substitui o processo de publicação do Power BI, o gerenciamento de permissões, o controle de acesso a fontes de dados nem os mecanismos de backup. Além disso, alguns arquivos ou alterações podem ser difíceis de revisar apenas pelo diff textual.

O projeto é um estudo prático de aprendizado e não pretende cobrir todos os cenários de colaboração ou implantação de soluções Power BI. A ideia principal é mostrar como um fluxo simples de Git pode trazer rastreabilidade e mais controle para a evolução de um projeto.

## Referências

- [Microsoft Learn — Power BI projects (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview)
- [Git — documentação oficial](https://git-scm.com/doc)
- [Git — git restore](https://git-scm.com/docs/git-restore)
- [Git — git revert](https://git-scm.com/docs/git-revert)
- [Git — git reset](https://git-scm.com/docs/git-reset)