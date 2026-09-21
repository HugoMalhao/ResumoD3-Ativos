# Focos em Aberto — Dashboard (base)

Dashboard HTML de **Ordens de Trabalho / focos em aberto** (todos os concelhos).

Publicação recomendada: **GitHub Pages** (conta gratuita).

---

## Conteúdo deste repositório

| Ficheiro | Função |
|----------|--------|
| `index.html` | Dashboard base completo (abrir no browser / GitHub Pages) |
| `README.md` | Este guia |

**Não** incluir o ficheiro `Focos_Aberto.csv` neste repositório (dados operacionais).

---

## Arquitetura de dados

```text
Focos_Aberto.csv  (Google Drive — substituir o mesmo ficheiro)
        ↓
Ponte Google Apps Script  (Web App /exec)
        ↓
index.html  (este dashboard — GitHub Pages ou local)
```

- O HTML **não** lê o Drive diretamente (o browser bloqueia — CORS).
- A **ponte** lê o CSV no Drive e devolve texto ao dashboard.
- Atualização: ao abrir + intervalo (5 / 15 / 30 / 60 min) ou **Atualizar agora**.
- Fallback: **Carregar CSV** manual + cache no browser.

### URL da ponte (já embutido no `index.html`)

```text
https://script.google.com/macros/s/AKfycbxPccM5PeYV4ztE4mZxT-r3o6p0xkZiFqVef9hbQBkBaCfz-hyJ0j01F0jmMTTvHMwL/exec
```

Se republicar a Web App e o URL mudar, atualize `APPS_SCRIPT_URL` no `index.html` (ou use **Config. ponte** no ecrã).

### CSV no Drive

- ID de referência: `16i0LgU-Bybbvwj9UOL9q5Xd7tz9k8LE6`
- Ao enviar dados novos: **Substituir** o mesmo ficheiro (não criar cópia com ID novo).

---

## Publicar no GitHub Pages (conta gratuita)

### 1. Criar repositório

1. [https://github.com/new](https://github.com/new)
2. Nome sugerido: `focos-em-aberto` (ou outro)
3. Visibilidade: **Public** (mais simples para Pages na conta free)
4. **Não** é obrigatório inicializar com README (este já existe)

### 2. Enviar ficheiros

No repositório → **Add file** → **Upload files**:

- `index.html`
- `README.md`

Commit para a branch `main` (ou `master`).

### 3. Ativar Pages

1. **Settings** → **Pages**
2. **Build and deployment** → Source: **Deploy from a branch**
3. Branch: `main` · pasta: `/ (root)`
4. **Save**
5. Esperar 1–2 minutos

### 4. Abrir o dashboard

```text
https://SEU_UTILIZADOR.github.io/focos-em-aberto/
```

(substitua `SEU_UTILIZADOR` e o nome do repo)

O link deve **executar** o dashboard (não mostrar código-fonte como no Google Drive).

---

## Utilização

| Ação | Resultado |
|------|-----------|
| Abrir o URL Pages | Carrega OT via ponte (todos os concelhos) |
| **Atualizar agora** | Novo pedido à ponte |
| **Intervalo** | Manual / 5 / 15 / 30 / 60 min |
| Filtros | Concelho · Estado · Executante · Idade (multi) |
| Clique na linha | Modal com todos os campos da OT |
| **Exportar CSV / PDF** | Vista filtrada atual |
| **Carregar CSV** | Fallback se a ponte falhar |
| **Config. ponte** | Só se o URL `/exec` mudar |

### Regras de negócio (base)

- **Concelho** = coluna Concelho (≠ Freguesia)
- Gráfico **OT por Concelho** = conta OT
- Gráfico **Focos por Executante** = soma focos (extraídos de `Descrição`)
- Coluna **Comentários** = campo Comentários (não Observations)
- Tabela ordenada por **Idade (dias)** decrescente
- KPIs sem idade média

---

## Atualizar dados no dia a dia

1. Gerar / exportar `Focos_Aberto.csv` (JUMP ou processo habitual).
2. No Google Drive → **Substituir** o ficheiro existente (mesmo ID).
3. No dashboard → **Atualizar agora** ou aguardar o intervalo.

Não é preciso republicar o GitHub quando só mudam os dados do CSV.  
Só é preciso novo upload do `index.html` quando alterar o **código** do dashboard.

---

## Segurança (importante)

- Repo **público** + HTML com URL da ponte: quem tiver o link do site pode, em princípio, obter o CSV via ponte.
- Trate o URL do GitHub Pages e o `/exec` como **uso interno** da equipa.
- Não publique o CSV com dados sensíveis no GitHub.
- A Web App deve manter o modelo que já funciona na vossa operação (acesso compatível com `fetch` do browser).

---

## Resolução de problemas

| Sintoma | O que verificar |
|---------|-----------------|
| `Failed to fetch` / falha na ponte | Web App ativa? URL `/exec` correto? Acesso da app? |
| Dados antigos | **Substituir** CSV no Drive + **Atualizar agora**; cache do Google pode atrasar 1–2 min |
| Pages 404 | Branch e pasta em Settings → Pages; ficheiro na raiz chama-se `index.html` |
| Aparece código em vez do dashboard | Está a abrir o ficheiro no Drive/preview — use o URL **github.io** |
| Ponte devolve erro | Conta que publicou o script ainda tem acesso ao CSV no Drive |

---

## Relação com o dashboard Caceiro

| Dashboard | Âmbito | Repo sugerido |
|-----------|--------|----------------|
| **Este (base)** | Todos os concelhos | `focos-em-aberto` |
| **Caceiro** | Só CONDEIXA-A-NOVA, SOURE, PENELA | `focos-abertos-caceiro` |

Ambos podem usar a **mesma ponte** e o **mesmo CSV** no Drive.  
O corte de concelhos no Caceiro é feito **só** no HTML Caceiro.

---

## Notas técnicas

- Stack: HTML + CSS + JavaScript (Chart.js, jsPDF, autoTable via CDN).
- Sem build, sem Node, sem servidor próprio além do GitHub Pages.
- Cache local: `localStorage` (`focosAbertoDashboard_v7`).
- CSV: separador `;`, encoding UTF-8 (BOM opcional).

---

## Checklist rápido de publicação

- [ ] Repo criado (público)
- [ ] `index.html` + `README.md` na raiz
- [ ] Pages ativo (`main` → `/root`)
- [ ] Abrir `https://USER.github.io/REPO/`
- [ ] Estado: **Origem: Ponte Apps Script** e OT > 0
- [ ] Partilhar só o link Pages com a equipa

---

*Dashboard base — Focos em Aberto. CSV no Google Drive · ponte Apps Script · UI no GitHub Pages.*
