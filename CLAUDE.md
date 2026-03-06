# CLAUDE.md — Projeto Dashboard Apps Script
> Este arquivo é lido automaticamente pelo Claude Code a cada sessão.
> Ele contém as diretrizes de desenvolvimento para este projeto HTML/GAS.

---

## 🧠 CONTEXTO DO PROJETO

Este projeto é um **dashboard web** desenvolvido com **Google Apps Script (GAS)**.
Os arquivos `.html` são servidos via `HtmlService` e se comunicam com o backend `.gs`
através de `google.script.run`. Os dados vêm de **Google Sheets**.

**Stack:**
- Backend: Google Apps Script (`.gs`)
- Frontend: HTML + CSS + JS (em arquivos `.html`)
- Deploy: `clasp push` via terminal
- Comunicação: `google.script.run` (assíncrono, sem Promises nativas)
- Dados: Google Sheets via `SpreadsheetApp`

---

## 🚨 REGRAS CRÍTICAS — NUNCA VIOLAR

1. **Nunca quebrar chamadas `google.script.run`** — são o único canal de comunicação frontend/backend
2. **Nunca usar `localStorage` ou `sessionStorage`** — não funcionam no ambiente GAS/iframe
3. **Nunca usar `fetch()` para chamar o backend** — use apenas `google.script.run`
4. **Nunca referenciar `window.location`** — o contexto de iframe do GAS quebra isso
5. **Evitar dependências CDN pesadas** — preferir libs leves; Tailwind CDN play é permitido
6. **Sempre testar compatibilidade com iframe** — o GAS serve HTML dentro de iframe sandboxed
7. **Nunca usar módulos ES6 (`import/export`)** — não são suportados no GAS HtmlService

---

## ⚡ PERFORMANCE — PADRÕES OBRIGATÓRIOS

### Injeção de dados inicial (zero round-trips)
```html
<!-- No .html, receba dados do servidor sem chamar google.script.run -->
<script>
  const INITIAL_DATA = <?= JSON.stringify(initialData) ?>;
</script>
```
```javascript
// No .gs, passe dados no template
function doGet() {
  const template = HtmlService.createTemplateFromFile('Index');
  template.initialData = getSheetData(); // chama Sheets uma vez
  return template.evaluate();
}
```

### Wrapper async/await para google.script.run
```javascript
// Sempre use este padrão — nunca chame google.script.run diretamente no código
function callGAS(fnName, ...args) {
  return new Promise((resolve, reject) => {
    google.script.run
      .withSuccessHandler(resolve)
      .withFailureHandler(reject)
      [fnName](...args);
  });
}

// Uso:
const data = await callGAS('getProjectData', projectId);
```

### Batching de chamadas
```javascript
// ERRADO — múltiplas chamadas ao backend
const users = await callGAS('getUsers');
const projects = await callGAS('getProjects');

// CORRETO — uma chamada que retorna tudo
const { users, projects } = await callGAS('getDashboardData');
```

### CacheService no backend
```javascript
// No .gs — sempre use cache para dados que não mudam frequentemente
function getSheetData() {
  const cache = CacheService.getScriptCache();
  const cached = cache.get('sheetData');
  if (cached) return JSON.parse(cached);
  
  const data = fetchFromSheet_(); // busca real
  cache.put('sheetData', JSON.stringify(data), 300); // 5 min
  return data;
}
```

### Debounce em filtros e buscas
```javascript
function debounce(fn, delay = 300) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

// Aplicar em inputs de filtro
searchInput.addEventListener('input', debounce(applyFilters, 350));
```

---

## 🎨 PADRÕES DE UI/UX

### Tema atual: Café ☕
Este projeto usa um tema visual de café (coffee shop). Sempre respeite a paleta:

```css
:root {
  /* Cores base */
  --cafe-dark:     #2C1A0E;
  --cafe-medium:   #3E2010;
  --cafe-caramel:  #C68642;
  --cafe-gold:     #D4A96A;
  --cafe-cream:    #FAF3E0;
  --cafe-beige:    #F5ECD7;
  --cafe-paper:    #EDE0CC;
  
  /* Textos */
  --text-primary:  #2C1A0E;
  --text-secondary:#6B4C2A;
  --text-light:    #FAF3E0;
  
  /* Status */
  --status-done:     #4A7C59; /* verde musgo */
  --status-progress: #C68642; /* âmbar/caramelo */
  --status-late:     #8B3A2A; /* vermelho terra */
  --status-pending:  #7A6652; /* marrom neutro */
  
  /* Sombras quentes */
  --shadow-sm:  0 2px 8px rgba(44,26,14,0.12);
  --shadow-md:  0 4px 16px rgba(44,26,14,0.18);
  --shadow-lg:  0 8px 32px rgba(44,26,14,0.24);
  
  /* Border radius */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 20px;
  
  /* Tipografia */
  --font-display: 'Playfair Display', serif;
  --font-body:    'DM Sans', sans-serif;
}
```

### Fontes via Google Fonts CDN
```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
```

### Ícones
Use **Font Awesome 6** via CDN:
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
```
Ícones temáticos preferidos: `fa-mug-hot`, `fa-coffee`, `fa-seedling`, `fa-fire-flame-curved`

---

## 🔔 SISTEMA DE FEEDBACK — PADRÃO OBRIGATÓRIO

Todo formulário, ação e carregamento DEVE ter feedback visual. Use este sistema:

```javascript
// Toast notification temática
function showToast(message, type = 'success') {
  const icons = { success: '☕', error: '⚠️', info: 'ℹ️', warning: '🫘' };
  const colors = {
    success: 'var(--cafe-caramel)',
    error:   'var(--status-late)',
    info:    'var(--cafe-medium)',
    warning: 'var(--cafe-gold)'
  };
  
  const toast = document.createElement('div');
  toast.className = 'toast';
  toast.innerHTML = `<span>${icons[type]}</span> ${message}`;
  toast.style.cssText = `
    position: fixed; bottom: 24px; right: 24px; z-index: 9999;
    background: ${colors[type]}; color: var(--cafe-cream);
    padding: 12px 20px; border-radius: var(--radius-md);
    font-family: var(--font-body); font-size: 14px; font-weight: 500;
    box-shadow: var(--shadow-md); animation: slideInToast 0.3s ease;
    display: flex; align-items: center; gap: 8px;
  `;
  
  document.body.appendChild(toast);
  setTimeout(() => toast.remove(), 3500);
}
```

### Loading state padrão
```javascript
function setLoading(button, isLoading) {
  if (isLoading) {
    button.dataset.originalText = button.innerHTML;
    button.innerHTML = '<i class="fa fa-mug-hot fa-spin"></i> Preparando...';
    button.disabled = true;
  } else {
    button.innerHTML = button.dataset.originalText;
    button.disabled = false;
  }
}
```

### Skeleton loading para tabelas
```html
<div class="skeleton-row" style="height:48px; background: linear-gradient(90deg, var(--cafe-paper) 25%, var(--cafe-beige) 50%, var(--cafe-paper) 75%); background-size: 200% 100%; animation: shimmer 1.5s infinite; border-radius: var(--radius-sm); margin-bottom: 8px;"></div>
```

---

## 📦 ESTRUTURA DE ARQUIVOS GAS

```
projeto/
├── Code.gs              # Backend principal
├── appsscript.json      # Manifest
├── PaginaReunioes.html  # Página principal
├── _CSS.html            # Estilos globais (incluído via include())
├── _JS.html             # Scripts globais (incluído via include())
└── CLAUDE.md            # Este arquivo
```

### Função include() obrigatória no Code.gs
```javascript
function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename).getContent();
}
```

### Uso nos templates HTML
```html
<!DOCTYPE html>
<html>
<head>
  <?!= include('_CSS') ?>
</head>
<body>
  <!-- conteúdo -->
  <?!= include('_JS') ?>
</body>
</html>
```

---

## 🪟 PADRÃO DE MODAIS

Modais devem comportar TODAS as informações. Use este padrão responsivo:

```css
.modal-overlay {
  position: fixed; inset: 0; z-index: 1000;
  background: rgba(44,26,14,0.6);
  backdrop-filter: blur(4px);
  display: flex; align-items: center; justify-content: center;
  animation: fadeIn 0.25s ease;
}

.modal-box {
  background: var(--cafe-cream);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  width: min(600px, 95vw);
  max-height: 90vh;
  display: flex; flex-direction: column;
  animation: slideUp 0.3s ease;
}

.modal-header {
  padding: 20px 24px 16px;
  border-bottom: 2px solid var(--cafe-paper);
  font-family: var(--font-display);
  color: var(--cafe-dark);
}

.modal-body {
  padding: 20px 24px;
  overflow-y: auto;    /* scroll interno — nunca deixa conteúdo cortado */
  flex: 1;
}

.modal-footer {
  padding: 16px 24px;
  border-top: 2px solid var(--cafe-paper);
  display: flex; gap: 12px; justify-content: flex-end;
}
```

---

## 🔍 PADRÃO DE FILTROS

```javascript
// Estado central de filtros
const filterState = {
  search: '',
  status: 'all',
  sector: 'all',
  dateFrom: null,
  dateTo: null
};

// Função única de aplicação (chame sempre que qualquer filtro mudar)
function applyFilters() {
  const filtered = allData.filter(item => {
    const matchSearch = !filterState.search || 
      item.name.toLowerCase().includes(filterState.search.toLowerCase());
    const matchStatus = filterState.status === 'all' || 
      item.status === filterState.status;
    const matchSector = filterState.sector === 'all' || 
      item.sector === filterState.sector;
    return matchSearch && matchStatus && matchSector;
  });
  
  renderTable(filtered);
  updateFilterCount(filtered.length);
}

// Vincular a todos os controles de filtro
document.querySelectorAll('[data-filter]').forEach(el => {
  el.addEventListener('change', e => {
    filterState[e.target.dataset.filter] = e.target.value;
    applyFilters();
  });
});
```

---

## ✅ CHECKLIST ANTES DE QUALQUER PUSH

Antes de rodar `clasp push`, verifique:

- [ ] `google.script.run` wrappado com Promise (padrão `callGAS`)
- [ ] Todo formulário tem validação + feedback visual
- [ ] Modais têm `overflow-y: auto` no body — sem conteúdo cortado
- [ ] Filtros usam estado centralizado + função única `applyFilters()`
- [ ] Dados iniciais injetados via template quando possível
- [ ] `CacheService` ativo no backend para dados frequentes
- [ ] Todas as ações têm estado de loading
- [ ] Toasts de sucesso e erro implementados
- [ ] Fontes e ícones carregados via CDN no `<head>`
- [ ] CSS com variáveis no `:root` (paleta café)
- [ ] Nenhum `localStorage` ou `sessionStorage`
- [ ] Nenhum `import/export` ES6

---

*Última atualização: gerado automaticamente para o projeto Dashboard de Reuniões ☕*
