# Focos em Aberto — Dashboard (base)

Dashboard web estático de **Ordens de Trabalho (OT)** em aberto, para todos os concelhos.

Esta pasta contém a **versão de referência** pronta para **GitHub Pages**.

## Ficheiros

| Ficheiro | Descrição |
|----------|-----------|
| `index.html` | Dashboard base (publicar na raiz do repositório) |
| `README.md` | Este guia |

## O que inclui

- Layout tema escuro (KPIs, gráficos Chart.js, tabela, modal)
- **Filtros multi** (Concelho, Estado, Executante, Idade): a lista **permanece aberta** para marcar várias opções; fecha com **Fechar**, clique fora, Escape ou ao abrir outro filtro
- Carga automática via **ponte Google Apps Script** (URL **já embutido** no HTML)
- Intervalo Manual / 5 / 15 / 30 / 60 min + **Atualizar agora**
- **Carregar CSV** manual (fallback)
- Exportar **CSV** e **PDF** (A4 landscape)
- Clique na linha → detalhe completo da OT
- Ordenação por **Idade (dias)** decrescente
- Cache local no browser se a ponte falhar

## Dados

| Peça | Onde fica |
|------|-----------|
| Dashboard (`index.html`) | **GitHub Pages** |
| CSV `Focos_Aberto.csv` | **Google Drive** (substituir no **mesmo** ficheiro) |
| Ponte Apps Script | **Google** (Web App `/exec`) |

**Não** é necessário colocar o CSV no GitHub.

Fluxo:

```text
CSV no Drive (Substituir, mesmo ID)
        ↓
Ponte Apps Script (/exec)
        ↓
Dashboard no GitHub Pages
```

## Publicar no GitHub Pages

1. Criar repositório público, por exemplo: `focos-em-aberto`
2. Fazer upload de `index.html` e `README.md` para a **raiz** (`main`)
3. **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: `main`
   - Folder: `/ (root)`
   - Save
4. Esperar 1–2 minutos
5. Abrir: `https://SEU_USER.github.io/focos-em-aberto/`

Os utilizadores **não** precisam de configurar a ponte: o URL `/exec` já está no `index.html`.

## Atualizar dados (dia a dia)

1. Exportar/enviar o novo `Focos_Aberto.csv` para o Drive
2. Escolher **Substituir** no ficheiro existente (manter o mesmo ID)
3. No dashboard: **Atualizar agora** ou aguardar o intervalo

Não é preciso republicar o Apps Script nem o GitHub quando só mudam os dados do CSV.

## Atualizar o dashboard (código)

1. Substituir o `index.html` no repositório (upload ou commit)
2. Esperar o Pages atualizar (por vezes 1–2 min; hard refresh no browser: Ctrl+F5)

## Ponte Apps Script

O `index.html` já inclui:

```text
https://script.google.com/macros/s/AKfycbxPccM5PeYV4ztE4mZxT-r3o6p0xkZiFqVef9hbQBkBaCfz-hyJ0j01F0jmMTTvHMwL/exec
```

Se a Web App for republicada e o URL mudar:

1. Editar no `index.html` a constante `APPS_SCRIPT_URL`
2. Voltar a fazer upload do `index.html`

### Requisitos da Web App

- Tipo: **Aplicação Web**
- Executar como: **Eu**
- Quem tem acesso: **Qualquer pessoa** (para o browser conseguir fazer `fetch` sem login)
- O script deve ler o CSV do Drive (ID de referência no HTML: `16i0LgU-Bybbvwj9UOL9q5Xd7tz9k8LE6`)

## Família de dashboards

| Repo / pasta | Âmbito |
|--------------|--------|
| **focos-em-aberto** (esta) | Todos os concelhos — **base de referência** |
| focos-abertos-caceiro | CONDEIXA-A-NOVA · SOURE · PENELA |
| focos-abertos-coimbra | Só COIMBRA |

Todos podem partilhar a **mesma ponte** e o **mesmo CSV** no Drive.

## Segurança (nota)

- O URL `/exec` fica visível no código-fonte do HTML (normal em site estático).
- Quem tiver o link do site e o da ponte pode obter o CSV.
- Tratar o link do Pages e o da ponte como **uso interno** da equipa.
- Preferir **não** publicar o CSV operacional no GitHub.

## Utilização rápida

1. Abrir o link Pages
2. Confirmar barra de estado: **Origem: Ponte Apps Script**
3. Usar filtros (multi-seleção sem fechar a cada clique)
4. **Limpar filtros** no header para repor tudo
5. Exportar CSV/PDF conforme necessário

## Resolução de problemas

| Sintoma | Verificação |
|---------|-------------|
| Falha ao atualizar / Failed to fetch | Rede; Web App ativa; acesso “Qualquer pessoa” |
| HTTP 403 | Republicar a Web App e autorizar de novo |
| Dados antigos | Substituir CSV no Drive + **Atualizar agora**; Ctrl+F5 no Pages |
| Lista de filtros fecha logo | Confirmar que está a usar **este** `index.html` (versão com painel persistente) |
| CSV manual | Usar **Carregar CSV** como fallback |

## Referência de desenvolvimento

A base local de trabalho validada é `focos-aberto-dashboard.html`.  
Este `index.html` é a cópia de publicação para GitHub Pages, com a **ponte incluída** e alinhada a essa referência (incluindo filtros multi sem fechar a cada seleção).
