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

## 🎯 PADRÃO DE QUALIDADE VISUAL — OBRIGATÓRIO

### Filosofia de Design

Cada pixel tem intenção. Nenhuma UI genérica será aceita neste projeto. O dashboard deve
transmitir a sensação de uma cafeteria sofisticada: tons quentes, texturas sutis, tipografia
elegante e micro-interações que fazem o usuário sentir confiança e prazer ao usar a ferramenta.

Pense em cada tela como uma página de cardápio de café especial: limpa, hierárquica, com
espaço para respirar, e com detalhes que mostram cuidado artesanal.

### Proibições Absolutas — NUNCA usar

1. **Fontes genéricas** como Arial, Helvetica, Times New Roman — use APENAS `var(--font-display)` e `var(--font-body)`
2. **Gradientes roxos, azuis ou neon** — o tema é café; tons quentes e terrosos apenas
3. **Cores hex diretas no CSS** — SEMPRE usar variáveis CSS do tema (`var(--cafe-*)`, `var(--status-*)`, `var(--text-*)`)
4. **Border radius de 0px ou 50%** — use apenas `var(--radius-sm)`, `var(--radius-md)`, `var(--radius-lg)`
5. **Sombras com preto puro** — use apenas `var(--shadow-sm)`, `var(--shadow-md)`, `var(--shadow-lg)` (sombras quentes)
6. **`!important`** — resolva especificidade pela ordem e seletores adequados
7. **Backgrounds brancos puros (#fff, white)** — use `var(--cafe-cream)` ou `var(--cafe-beige)` como base
8. **Ícones genéricos sem contexto** — prefira ícones temáticos de café quando possível
9. **Textos cinza frio (#666, #999, gray)** — use `var(--text-secondary)` para textos secundários
10. **Bordas cinza (#ddd, #ccc, #e0e0e0)** — use `var(--cafe-paper)` ou `var(--cafe-beige)` para divisórias
11. **Componentes sem estado de hover** — todo elemento interativo DEVE ter transição de hover
12. **Inputs sem estado de focus visível** — acessibilidade e feedback são obrigatórios
13. **Botões sem estado disabled** — nunca entregar botão sem prever desabilitação
14. **Tabelas sem zebra-striping** — linhas alternadas são obrigatórias para legibilidade

### Padrão por Componente

**Botões:**
- Sempre ter 5 estados visuais: default, hover, active, focus, disabled
- Padding mínimo: `10px 20px`
- Usar `var(--radius-sm)` como padrão
- Transição suave: `transition: all 0.2s ease`
- Ícone à esquerda quando presente, com `gap: 8px`
- Estado de loading substitui texto por spinner + "Preparando..."
- Nunca aparentar clicável quando disabled (opacity + cursor default)

**Inputs e Formulários:**
- Todo input tem `label` visível ou `aria-label`
- Border padrão: `2px solid var(--cafe-paper)`
- Focus: border muda para `var(--cafe-caramel)` com glow sutil
- Erro: border muda para `var(--status-late)` com mensagem abaixo
- Sucesso: border muda para `var(--status-done)` momentaneamente
- Placeholder com cor `var(--text-secondary)` e opacidade 0.6
- Altura mínima: `44px` (acessibilidade touch)

**Tabelas:**
- Header com `background: var(--cafe-dark)` e `color: var(--text-light)`
- Zebra: linhas ímpares com `background: var(--cafe-beige)`
- Hover de linha: `background: var(--cafe-paper)` com transição
- Células com padding `12px 16px`
- Texto numérico alinhado à direita
- Coluna de ações sempre à direita, com ícones em botões ghost
- Borda interna: `1px solid var(--cafe-paper)`

**Cards:**
- Background: `var(--cafe-cream)`
- Border: `1px solid var(--cafe-paper)`
- Radius: `var(--radius-md)`
- Shadow padrão: `var(--shadow-sm)`, hover: `var(--shadow-md)`
- Padding interno: `20px 24px`
- Header do card com `var(--font-display)`, body com `var(--font-body)`
- Transição no hover: `transform: translateY(-2px)`

**Modais:**
- Overlay com `backdrop-filter: blur(4px)`
- Entrada animada com `slideUp 0.3s ease`
- Saída animada com `fadeOut 0.2s ease`
- Scroll interno no body — NUNCA cortar conteúdo
- Botão de fechar visível no header (X) e clique no overlay
- Footer com botões alinhados à direita

**Badges de Status:**
- Formato: ícone + texto, com padding `4px 12px`
- Radius: `var(--radius-lg)` (pill shape)
- Cada status tem fundo com opacidade 15% da cor + texto na cor sólida
- Ícones: done=check, progress=clock, late=exclamation, pending=hourglass
- Nunca usar apenas cor para comunicar status — sempre ter ícone + texto

**Navegação/Sidebar:**
- Item ativo com `background: var(--cafe-paper)` e `border-left: 3px solid var(--cafe-caramel)`
- Hover com `background: var(--cafe-beige)`
- Ícones com 20px, texto com `var(--font-body)` peso 500
- Transição suave no hover e no active

**Formulários:**
- Campos agrupados com `gap: 16px` entre eles
- Labels com peso 600, tamanho 13px, cor `var(--text-secondary)`, em maiúsculas
- Mensagens de erro em `var(--status-late)`, tamanho 12px, abaixo do campo
- Botão submit sempre no canto inferior direito do form
- Validação visual em tempo real (não só no submit)

### Hierarquia Tipográfica Obrigatória

```css
/* Títulos — usar var(--font-display) */
h1 { font-size: 28px; font-weight: 700; line-height: 1.2; letter-spacing: -0.02em; color: var(--cafe-dark); }
h2 { font-size: 22px; font-weight: 700; line-height: 1.3; letter-spacing: -0.01em; color: var(--cafe-dark); }
h3 { font-size: 18px; font-weight: 600; line-height: 1.4; color: var(--cafe-dark); }
h4 { font-size: 16px; font-weight: 600; line-height: 1.4; color: var(--text-primary); }
h5 { font-size: 14px; font-weight: 600; line-height: 1.5; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.05em; }
h6 { font-size: 12px; font-weight: 600; line-height: 1.5; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 0.08em; }

/* Body — usar var(--font-body) */
/* Texto padrão:  14px, peso 400, line-height 1.6 */
/* Texto pequeno: 12px, peso 400, line-height 1.5 */
/* Texto grande:  16px, peso 400, line-height 1.6 */
/* Labels:        13px, peso 600, line-height 1.4, uppercase */
/* Captions:      11px, peso 400, line-height 1.4, text-secondary */
```

### Micro-Interações Obrigatórias

Todo componente interativo DEVE ter estas transições:

```css
/* Transição base para todos os elementos interativos */
.interactive {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Hover em cards — elevação sutil */
.card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

/* Hover em linhas de tabela */
.table__row:hover {
  background: var(--cafe-paper);
}

/* Focus visível em inputs */
.input:focus {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.15);
  outline: none;
}

/* Active em botões — compressão sutil */
.btn:active {
  transform: scale(0.97);
}

/* Disabled — dessaturação */
.btn:disabled,
.input:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
```

### Sistema de Animações de Entrada (Keyframes)

```css
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@keyframes fadeOut {
  from { opacity: 1; }
  to   { opacity: 0; }
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes slideDown {
  from { opacity: 0; transform: translateY(-20px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes slideInRight {
  from { opacity: 0; transform: translateX(20px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes slideInToast {
  from { opacity: 0; transform: translateX(100px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes slideOutToast {
  from { opacity: 1; transform: translateX(0); }
  to   { opacity: 0; transform: translateX(100px); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.9); }
  to   { opacity: 1; transform: scale(1); }
}

@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50%      { opacity: 0.5; }
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

@keyframes bounceIn {
  0%   { opacity: 0; transform: scale(0.3); }
  50%  { transform: scale(1.05); }
  70%  { transform: scale(0.95); }
  100% { opacity: 1; transform: scale(1); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  10%, 30%, 50%, 70%, 90% { transform: translateX(-4px); }
  20%, 40%, 60%, 80% { transform: translateX(4px); }
}
```

### Checklist Visual — Verificar Antes de Entregar Qualquer HTML/CSS

- [ ] Todas as cores usam variáveis CSS do tema (nenhum hex direto)
- [ ] Todas as fontes são `var(--font-display)` ou `var(--font-body)`
- [ ] Todos os radius usam `var(--radius-*)`
- [ ] Todas as sombras usam `var(--shadow-*)`
- [ ] Botões têm 5 estados: default, hover, active, focus, disabled
- [ ] Inputs têm 4 estados: default, focus, error, disabled
- [ ] Tabelas têm zebra-striping e hover de linha
- [ ] Cards têm hover com elevação
- [ ] Modais têm animação de entrada e scroll interno
- [ ] Badges de status usam ícone + texto + cor temática
- [ ] Hierarquia tipográfica H1→H6 respeita os tamanhos definidos
- [ ] Todo texto placeholder é legível (`opacity: 0.6`)
- [ ] Altura mínima de toque é 44px em elementos clicáveis
- [ ] Contraste de texto atende WCAG AA (mínimo 4.5:1)
- [ ] Nenhum `!important` no CSS
- [ ] Nenhum background branco puro (#fff)
- [ ] Feedback de loading em toda ação assíncrona
- [ ] Toast de sucesso e erro em toda operação CRUD
- [ ] Transições suaves em todo hover (mínimo 0.2s)
- [ ] Animação de entrada em elementos que aparecem dinamicamente
- [ ] Empty states estilizados quando listas estão vazias
- [ ] Skeleton loading para dados que demoram a carregar
- [ ] Formulários com validação visual inline
- [ ] Scroll interno em modais — conteúdo NUNCA cortado
- [ ] Ícones do Font Awesome carregados e visíveis

---

## 🖌️ PADRÃO DE CÓDIGO CSS

### Ordem de Propriedades CSS

Sempre escreva propriedades CSS nesta ordem:

```css
.componente {
  /* 1. Posicionamento */
  position: relative;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1;
  
  /* 2. Display e Layout */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  grid-template-columns: 1fr 1fr;
  
  /* 3. Box Model */
  width: 100%;
  max-width: 600px;
  height: auto;
  min-height: 44px;
  margin: 0 auto;
  padding: 12px 16px;
  overflow: hidden;
  
  /* 4. Tipografia */
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  line-height: 1.6;
  letter-spacing: 0;
  text-align: left;
  text-transform: none;
  color: var(--text-primary);
  
  /* 5. Visual */
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  opacity: 1;
  
  /* 6. Animação */
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  animation: fadeIn 0.3s ease;
  cursor: pointer;
}
```

### Nomenclatura de Classes — BEM Adaptado

```css
/* Bloco: componente principal */
.card { }
.table { }
.modal { }
.nav { }

/* Elemento: parte interna do bloco (separador: __) */
.card__header { }
.card__body { }
.card__footer { }
.table__row { }
.table__cell { }
.modal__overlay { }

/* Modificador: variação do bloco ou elemento (separador: --) */
.btn--primary { }
.btn--secondary { }
.btn--ghost { }
.btn--danger { }
.btn--loading { }
.card--kpi { }
.badge--done { }
.badge--late { }
.input--error { }
.input--success { }

/* Estado: classes utilitárias de estado (prefixo: is-) */
.is-active { }
.is-disabled { }
.is-loading { }
.is-hidden { }
.is-visible { }
.is-open { }
```

### Regras para Variáveis CSS

**Quando REUTILIZAR uma variável existente:**
- A cor desejada existe na paleta café → use `var(--cafe-*)`
- O status tem cor definida → use `var(--status-*)`
- O radius está entre sm/md/lg → use `var(--radius-*)`
- A sombra está entre sm/md/lg → use `var(--shadow-*)`
- A fonte é display ou body → use `var(--font-*)`

**Quando CRIAR uma nova variável:**
- Novo breakpoint específico do layout: `--bp-sidebar: 768px`
- Nova duração de animação reutilizada: `--duration-fast: 150ms`
- Espaçamento padrão de seção: `--spacing-section: 32px`
- Largura máxima do conteúdo: `--content-max-width: 1200px`

**NUNCA criar variável para:**
- Uma cor que já existe na paleta (duplicação)
- Um valor usado apenas uma vez (inline é mais claro)
- Sobrescrever uma variável existente com `!important`

---

## 🔄 PROCESSO DE TRABALHO PARA UI

### ANTES de codar qualquer tela:

1. **Identificar hierarquia visual** — qual é o elemento principal? (ex: tabela de reuniões é protagonista, filtros são coadjuvantes)
2. **Definir fluxo do olhar** — de onde o olho entra? (título → métricas → tabela → ações)
3. **Mapear estados** — quais são todos os estados possíveis? (carregando, vazio, com dados, erro, filtrando)
4. **Listar componentes necessários** — conferir se já existem no `.claude/rules/frontend-design.md`

### DURANTE a codificação:

1. **A cada componente novo**, verificar se usa variáveis CSS do tema café
2. **A cada interação**, verificar se tem hover, focus e feedback visual
3. **A cada texto**, verificar se usa a fonte e peso corretos da hierarquia
4. **A cada cor**, verificar se é variável CSS (nunca hex direto)
5. **A cada input**, verificar se tem label e estados de validação

### DEPOIS de codar:

1. **Checklist visual** — rodar todos os 25 itens da checklist acima
2. **Acessibilidade mínima:**
   - Todo input tem `label` ou `aria-label`
   - Contraste mínimo 4.5:1 (os tons café atendem naturalmente)
   - Focus visível em todos os interativos
   - Semântica HTML (`button`, `table`, `nav`, `main`, `section`)
   - `aria-live="polite"` em áreas que atualizam dinamicamente
3. **Responsividade:**
   - Testar em largura de iframe GAS (~100% do viewport disponível)
   - Tabelas com `overflow-x: auto` para scroll horizontal em telas pequenas
   - Modais com `width: min(600px, 95vw)`
   - Grids com `repeat(auto-fill, minmax(280px, 1fr))`
4. **Performance visual:**
   - Skeleton loading enquanto dados carregam
   - Transições CSS (nunca mudanças abruptas de estado)
   - Nenhum layout shift ao carregar dados

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
- [ ] Checklist visual (25 itens) aprovada
- [ ] Acessibilidade mínima verificada
- [ ] Empty states implementados para listas vazias
- [ ] Skeleton loading implementado para áreas de dados

---

*Última atualização: gerado automaticamente para o projeto Dashboard de Reuniões ☕*
