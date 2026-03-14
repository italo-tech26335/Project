# Frontend Design — Componentes CSS Tema Café ☕
> Regra carregada automaticamente pelo Claude Code.
> Todos os componentes seguem a paleta café definida no CLAUDE.md.
> Copie e cole diretamente — todo código é 100% funcional.

---

## 📐 RESET E BASE GLOBAL

Sempre incluir este reset no `_CSS.html` antes de qualquer componente:

```css
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 16px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 400;
  line-height: 1.6;
  color: var(--text-primary);
  background: var(--cafe-beige);
  overflow-x: hidden;
}

h1, h2, h3, h4 {
  font-family: var(--font-display);
  color: var(--cafe-dark);
}

h5, h6 {
  font-family: var(--font-body);
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

a {
  color: var(--cafe-caramel);
  text-decoration: none;
  transition: color 0.2s ease;
}

a:hover {
  color: var(--cafe-medium);
}

img {
  max-width: 100%;
  display: block;
}

button {
  font-family: var(--font-body);
  cursor: pointer;
  border: none;
  background: none;
}

input, select, textarea {
  font-family: var(--font-body);
  font-size: 14px;
}

table {
  border-collapse: collapse;
  width: 100%;
}

:focus-visible {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}
```

---

## 🔘 BOTÕES

### Botão Primário

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 20px;
  min-height: 44px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 600;
  line-height: 1.4;
  text-decoration: none;
  border: 2px solid transparent;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  white-space: nowrap;
}

.btn--primary {
  background: var(--cafe-caramel);
  color: var(--text-light);
  border-color: var(--cafe-caramel);
}

.btn--primary:hover {
  background: var(--cafe-medium);
  border-color: var(--cafe-medium);
  box-shadow: var(--shadow-sm);
  transform: translateY(-1px);
}

.btn--primary:active {
  background: var(--cafe-dark);
  border-color: var(--cafe-dark);
  transform: scale(0.97);
  box-shadow: none;
}

.btn--primary:focus-visible {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}

.btn--primary:disabled,
.btn--primary.is-disabled {
  background: var(--cafe-paper);
  border-color: var(--cafe-paper);
  color: var(--text-secondary);
  opacity: 0.6;
  cursor: not-allowed;
  pointer-events: none;
  transform: none;
  box-shadow: none;
}
```

### Botão Secundário

```css
.btn--secondary {
  background: transparent;
  color: var(--cafe-caramel);
  border-color: var(--cafe-caramel);
}

.btn--secondary:hover {
  background: var(--cafe-caramel);
  color: var(--text-light);
  box-shadow: var(--shadow-sm);
  transform: translateY(-1px);
}

.btn--secondary:active {
  background: var(--cafe-medium);
  border-color: var(--cafe-medium);
  color: var(--text-light);
  transform: scale(0.97);
}

.btn--secondary:focus-visible {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}

.btn--secondary:disabled,
.btn--secondary.is-disabled {
  border-color: var(--cafe-paper);
  color: var(--text-secondary);
  opacity: 0.6;
  cursor: not-allowed;
  pointer-events: none;
}
```

### Botão Ghost

```css
.btn--ghost {
  background: transparent;
  color: var(--text-secondary);
  border-color: transparent;
  padding: 8px 12px;
}

.btn--ghost:hover {
  background: var(--cafe-paper);
  color: var(--cafe-dark);
}

.btn--ghost:active {
  background: var(--cafe-beige);
  transform: scale(0.97);
}

.btn--ghost:focus-visible {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}

.btn--ghost:disabled,
.btn--ghost.is-disabled {
  color: var(--cafe-paper);
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
```

### Botão Danger

```css
.btn--danger {
  background: var(--status-late);
  color: var(--text-light);
  border-color: var(--status-late);
}

.btn--danger:hover {
  background: var(--cafe-dark);
  border-color: var(--cafe-dark);
  box-shadow: var(--shadow-sm);
  transform: translateY(-1px);
}

.btn--danger:active {
  background: var(--cafe-dark);
  transform: scale(0.97);
}

.btn--danger:focus-visible {
  outline: 2px solid var(--status-late);
  outline-offset: 2px;
}

.btn--danger:disabled,
.btn--danger.is-disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
  transform: none;
}
```

### Botão com Ícone

```html
<!-- Estrutura HTML padrão -->
<button class="btn btn--primary">
  <i class="fa-solid fa-plus"></i>
  Nova Reunião
</button>

<button class="btn btn--secondary">
  <i class="fa-solid fa-filter"></i>
  Filtrar
</button>

<!-- Botão somente ícone -->
<button class="btn btn--ghost btn--icon" aria-label="Editar">
  <i class="fa-solid fa-pen"></i>
</button>
```

```css
.btn--icon {
  padding: 8px;
  min-height: 36px;
  min-width: 36px;
  border-radius: var(--radius-sm);
}

.btn--icon i {
  font-size: 14px;
}
```

### Botão Loading State

```css
.btn.is-loading {
  position: relative;
  color: transparent;
  pointer-events: none;
}

.btn.is-loading::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 18px;
  height: 18px;
  margin: -9px 0 0 -9px;
  border: 2px solid var(--cafe-cream);
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

.btn--secondary.is-loading::after {
  border-color: var(--cafe-caramel);
  border-top-color: transparent;
}
```

### Grupo de Botões

```css
.btn-group {
  display: flex;
  gap: 8px;
  align-items: center;
  flex-wrap: wrap;
}

.btn-group--right {
  justify-content: flex-end;
}

.btn-group--between {
  justify-content: space-between;
}
```

---

## 📝 INPUTS E FORMULÁRIOS

### Input Text

```css
.form-field {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-field__label {
  font-family: var(--font-body);
  font-size: 13px;
  font-weight: 600;
  line-height: 1.4;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.form-field__label--required::after {
  content: ' *';
  color: var(--status-late);
}

.input {
  display: block;
  width: 100%;
  min-height: 44px;
  padding: 10px 14px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 400;
  line-height: 1.5;
  color: var(--text-primary);
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: var(--radius-sm);
  outline: none;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.input::placeholder {
  color: var(--text-secondary);
  opacity: 0.6;
}

.input:hover {
  border-color: var(--cafe-gold);
}

.input:focus {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.15);
}

.input--error,
.form-field.is-error .input {
  border-color: var(--status-late);
  box-shadow: 0 0 0 3px rgba(139,58,42,0.1);
}

.input--success,
.form-field.is-success .input {
  border-color: var(--status-done);
  box-shadow: 0 0 0 3px rgba(74,124,89,0.1);
}

.input:disabled {
  background: var(--cafe-beige);
  color: var(--text-secondary);
  opacity: 0.6;
  cursor: not-allowed;
}

.form-field__error {
  font-size: 12px;
  font-weight: 500;
  color: var(--status-late);
  display: none;
  align-items: center;
  gap: 4px;
}

.form-field.is-error .form-field__error {
  display: flex;
}

.form-field__hint {
  font-size: 12px;
  color: var(--text-secondary);
  opacity: 0.8;
}
```

### Select

```css
.select-wrapper {
  position: relative;
  display: block;
}

.select {
  display: block;
  width: 100%;
  min-height: 44px;
  padding: 10px 40px 10px 14px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 400;
  color: var(--text-primary);
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: var(--radius-sm);
  outline: none;
  cursor: pointer;
  appearance: none;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.select-wrapper::after {
  content: '\f078';
  font-family: 'Font Awesome 6 Free';
  font-weight: 900;
  font-size: 12px;
  position: absolute;
  top: 50%;
  right: 14px;
  transform: translateY(-50%);
  color: var(--text-secondary);
  pointer-events: none;
}

.select:hover {
  border-color: var(--cafe-gold);
}

.select:focus {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.15);
}

.select:disabled {
  background: var(--cafe-beige);
  opacity: 0.6;
  cursor: not-allowed;
}
```

### Textarea

```css
.textarea {
  display: block;
  width: 100%;
  min-height: 100px;
  padding: 12px 14px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 400;
  line-height: 1.6;
  color: var(--text-primary);
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: var(--radius-sm);
  outline: none;
  resize: vertical;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.textarea:hover {
  border-color: var(--cafe-gold);
}

.textarea:focus {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.15);
}

.textarea:disabled {
  background: var(--cafe-beige);
  opacity: 0.6;
  cursor: not-allowed;
  resize: none;
}
```

### Checkbox

```css
.checkbox {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  font-family: var(--font-body);
  font-size: 14px;
  color: var(--text-primary);
  user-select: none;
}

.checkbox__input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.checkbox__mark {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: 4px;
  transition: all 0.2s ease;
}

.checkbox__mark::after {
  content: '\f00c';
  font-family: 'Font Awesome 6 Free';
  font-weight: 900;
  font-size: 11px;
  color: var(--text-light);
  opacity: 0;
  transform: scale(0);
  transition: all 0.15s ease;
}

.checkbox:hover .checkbox__mark {
  border-color: var(--cafe-gold);
}

.checkbox__input:focus-visible + .checkbox__mark {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}

.checkbox__input:checked + .checkbox__mark {
  background: var(--cafe-caramel);
  border-color: var(--cafe-caramel);
}

.checkbox__input:checked + .checkbox__mark::after {
  opacity: 1;
  transform: scale(1);
}

.checkbox__input:disabled + .checkbox__mark {
  opacity: 0.5;
  cursor: not-allowed;
}

.checkbox__input:disabled ~ .checkbox__label {
  opacity: 0.5;
}
```

```html
<!-- Estrutura HTML padrão -->
<label class="checkbox">
  <input type="checkbox" class="checkbox__input">
  <span class="checkbox__mark"></span>
  <span class="checkbox__label">Opção de exemplo</span>
</label>
```

### Radio Button

```css
.radio {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  font-family: var(--font-body);
  font-size: 14px;
  color: var(--text-primary);
  user-select: none;
}

.radio__input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.radio__mark {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: 50%;
  transition: all 0.2s ease;
}

.radio__mark::after {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--text-light);
  opacity: 0;
  transform: scale(0);
  transition: all 0.15s ease;
}

.radio:hover .radio__mark {
  border-color: var(--cafe-gold);
}

.radio__input:focus-visible + .radio__mark {
  outline: 2px solid var(--cafe-caramel);
  outline-offset: 2px;
}

.radio__input:checked + .radio__mark {
  background: var(--cafe-caramel);
  border-color: var(--cafe-caramel);
}

.radio__input:checked + .radio__mark::after {
  opacity: 1;
  transform: scale(1);
}

.radio__input:disabled + .radio__mark {
  opacity: 0.5;
  cursor: not-allowed;
}
```

```html
<label class="radio">
  <input type="radio" name="grupo" class="radio__input">
  <span class="radio__mark"></span>
  <span class="radio__label">Opção A</span>
</label>
```

### Layout de Formulário

```css
.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form__row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.form__row--full {
  grid-template-columns: 1fr;
}

.form__actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  padding-top: 8px;
  border-top: 1px solid var(--cafe-paper);
  margin-top: 4px;
}

@media (max-width: 600px) {
  .form__row {
    grid-template-columns: 1fr;
  }
}
```

---

## 📊 TABELA COMPLETA

```css
.table-container {
  background: var(--cafe-cream);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  overflow: hidden;
}

.table-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 16px 20px;
  border-bottom: 1px solid var(--cafe-paper);
  flex-wrap: wrap;
}

.table-toolbar__search {
  position: relative;
  flex: 1;
  max-width: 320px;
  min-width: 200px;
}

.table-toolbar__search-input {
  width: 100%;
  padding: 8px 12px 8px 36px;
  min-height: 38px;
  font-size: 13px;
  background: var(--cafe-beige);
  border: 2px solid transparent;
  border-radius: var(--radius-sm);
  outline: none;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
  color: var(--text-primary);
  font-family: var(--font-body);
}

.table-toolbar__search-input:focus {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.1);
  background: var(--cafe-cream);
}

.table-toolbar__search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-secondary);
  font-size: 13px;
  pointer-events: none;
}

.table-scroll {
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table__head {
  background: var(--cafe-dark);
}

.table__head-cell {
  padding: 12px 16px;
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 600;
  line-height: 1.4;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--cafe-gold);
  text-align: left;
  white-space: nowrap;
  cursor: default;
  user-select: none;
  border-bottom: 2px solid var(--cafe-medium);
}

.table__head-cell--sortable {
  cursor: pointer;
  transition: color 0.2s ease;
}

.table__head-cell--sortable:hover {
  color: var(--text-light);
}

.table__head-cell--sorted-asc::after {
  content: ' \f077';
  font-family: 'Font Awesome 6 Free';
  font-weight: 900;
  font-size: 10px;
  margin-left: 4px;
}

.table__head-cell--sorted-desc::after {
  content: ' \f078';
  font-family: 'Font Awesome 6 Free';
  font-weight: 900;
  font-size: 10px;
  margin-left: 4px;
}

.table__head-cell--right {
  text-align: right;
}

.table__head-cell--center {
  text-align: center;
}

.table__row {
  border-bottom: 1px solid var(--cafe-paper);
  transition: background 0.15s ease;
}

.table__row:nth-child(even) {
  background: var(--cafe-beige);
}

.table__row:hover {
  background: var(--cafe-paper);
}

.table__cell {
  padding: 12px 16px;
  font-size: 14px;
  color: var(--text-primary);
  vertical-align: middle;
}

.table__cell--right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

.table__cell--center {
  text-align: center;
}

.table__cell--actions {
  text-align: right;
  white-space: nowrap;
}

.table__cell--actions .btn--ghost {
  padding: 6px 8px;
  min-height: auto;
}

.table__cell--truncate {
  max-width: 200px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Paginação */
.table-pagination {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 20px;
  border-top: 1px solid var(--cafe-paper);
  font-size: 13px;
  color: var(--text-secondary);
}

.table-pagination__info {
  font-weight: 500;
}

.table-pagination__controls {
  display: flex;
  align-items: center;
  gap: 4px;
}

.table-pagination__btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: var(--radius-sm);
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
  background: transparent;
  border: none;
  cursor: pointer;
  transition: all 0.15s ease;
  font-family: var(--font-body);
}

.table-pagination__btn:hover {
  background: var(--cafe-paper);
  color: var(--cafe-dark);
}

.table-pagination__btn.is-active {
  background: var(--cafe-caramel);
  color: var(--text-light);
}

.table-pagination__btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
```

```html
<!-- Estrutura HTML padrão da tabela -->
<div class="table-container">
  <div class="table-toolbar">
    <div class="table-toolbar__search">
      <i class="fa-solid fa-search table-toolbar__search-icon"></i>
      <input type="text" class="table-toolbar__search-input" placeholder="Buscar...">
    </div>
    <div class="btn-group">
      <button class="btn btn--secondary">
        <i class="fa-solid fa-filter"></i> Filtrar
      </button>
      <button class="btn btn--primary">
        <i class="fa-solid fa-plus"></i> Novo
      </button>
    </div>
  </div>
  
  <div class="table-scroll">
    <table class="table">
      <thead class="table__head">
        <tr>
          <th class="table__head-cell table__head-cell--sortable">Título</th>
          <th class="table__head-cell">Status</th>
          <th class="table__head-cell table__head-cell--right">Valor</th>
          <th class="table__head-cell table__head-cell--center">Ações</th>
        </tr>
      </thead>
      <tbody id="tableBody">
        <!-- Linhas renderizadas via JS -->
      </tbody>
    </table>
  </div>
  
  <div class="table-pagination">
    <span class="table-pagination__info">Mostrando 1-10 de 42</span>
    <div class="table-pagination__controls">
      <button class="table-pagination__btn" disabled><i class="fa-solid fa-chevron-left"></i></button>
      <button class="table-pagination__btn is-active">1</button>
      <button class="table-pagination__btn">2</button>
      <button class="table-pagination__btn">3</button>
      <button class="table-pagination__btn"><i class="fa-solid fa-chevron-right"></i></button>
    </div>
  </div>
</div>
```

---

## 🃏 CARDS

### Card Simples

```css
.card {
  background: var(--cafe-cream);
  border: 1px solid var(--cafe-paper);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  padding: 20px 24px;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}
```

### Card com Header

```css
.card--header {
  padding: 0;
}

.card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  border-bottom: 1px solid var(--cafe-paper);
}

.card__title {
  font-family: var(--font-display);
  font-size: 16px;
  font-weight: 600;
  color: var(--cafe-dark);
}

.card__subtitle {
  font-size: 12px;
  color: var(--text-secondary);
  margin-top: 2px;
}

.card__header-actions {
  display: flex;
  gap: 4px;
}

.card__body {
  padding: 20px;
}

.card__footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 8px;
  padding: 12px 20px;
  border-top: 1px solid var(--cafe-paper);
}
```

### Card KPI (Métrica)

```css
.card--kpi {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 20px 24px;
  position: relative;
  overflow: hidden;
}

.card--kpi::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 100%;
  background: var(--cafe-caramel);
  border-radius: 0 4px 4px 0;
}

.card--kpi .card__kpi-label {
  font-size: 12px;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.card--kpi .card__kpi-value {
  font-family: var(--font-display);
  font-size: 32px;
  font-weight: 700;
  color: var(--cafe-dark);
  line-height: 1.1;
}

.card--kpi .card__kpi-change {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 12px;
  font-weight: 600;
}

.card--kpi .card__kpi-change--up {
  color: var(--status-done);
}

.card--kpi .card__kpi-change--down {
  color: var(--status-late);
}

.card--kpi .card__kpi-icon {
  position: absolute;
  top: 16px;
  right: 20px;
  font-size: 24px;
  color: var(--cafe-paper);
}
```

```html
<!-- Card KPI -->
<div class="card card--kpi">
  <i class="fa-solid fa-mug-hot card__kpi-icon"></i>
  <span class="card__kpi-label">Total de Reuniões</span>
  <span class="card__kpi-value">128</span>
  <span class="card__kpi-change card__kpi-change--up">
    <i class="fa-solid fa-arrow-up"></i> 12% vs mês anterior
  </span>
</div>
```

### Grid de Cards KPI

```css
.kpi-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}
```

---

## 🏷️ BADGES DE STATUS

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 12px;
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 600;
  line-height: 1.4;
  border-radius: var(--radius-lg);
  white-space: nowrap;
}

.badge--done {
  background: rgba(74,124,89,0.12);
  color: var(--status-done);
}

.badge--progress {
  background: rgba(198,134,66,0.15);
  color: var(--status-progress);
}

.badge--late {
  background: rgba(139,58,42,0.12);
  color: var(--status-late);
}

.badge--pending {
  background: rgba(122,102,82,0.12);
  color: var(--status-pending);
}

.badge__icon {
  font-size: 10px;
}
```

```html
<span class="badge badge--done">
  <i class="fa-solid fa-check badge__icon"></i> Concluído
</span>
<span class="badge badge--progress">
  <i class="fa-solid fa-clock badge__icon"></i> Em Andamento
</span>
<span class="badge badge--late">
  <i class="fa-solid fa-exclamation badge__icon"></i> Atrasado
</span>
<span class="badge badge--pending">
  <i class="fa-solid fa-hourglass-half badge__icon"></i> Pendente
</span>
```

### Geração via JS

```javascript
function renderBadge(status) {
  const map = {
    done:     { label: 'Concluído',    icon: 'fa-check',          css: 'badge--done' },
    progress: { label: 'Em Andamento', icon: 'fa-clock',          css: 'badge--progress' },
    late:     { label: 'Atrasado',     icon: 'fa-exclamation',    css: 'badge--late' },
    pending:  { label: 'Pendente',     icon: 'fa-hourglass-half', css: 'badge--pending' }
  };
  const s = map[status] || map.pending;
  return `<span class="badge ${s.css}"><i class="fa-solid ${s.icon} badge__icon"></i> ${s.label}</span>`;
}
```

---

## 🪟 MODAL COMPLETO

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(44,26,14,0.6);
  backdrop-filter: blur(4px);
  animation: fadeIn 0.25s ease;
  opacity: 1;
}

.modal-overlay.is-closing {
  animation: fadeOut 0.2s ease forwards;
}

.modal-box {
  display: flex;
  flex-direction: column;
  width: min(600px, 95vw);
  max-height: 90vh;
  background: var(--cafe-cream);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  animation: slideUp 0.3s ease;
}

.modal-overlay.is-closing .modal-box {
  animation: fadeOut 0.2s ease forwards;
}

.modal-box--wide {
  width: min(800px, 95vw);
}

.modal-box--narrow {
  width: min(420px, 95vw);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 24px 16px;
  border-bottom: 2px solid var(--cafe-paper);
}

.modal-header__title {
  font-family: var(--font-display);
  font-size: 20px;
  font-weight: 700;
  color: var(--cafe-dark);
}

.modal-header__close {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: var(--radius-sm);
  color: var(--text-secondary);
  font-size: 16px;
  cursor: pointer;
  background: transparent;
  border: none;
  transition: all 0.15s ease;
}

.modal-header__close:hover {
  background: var(--cafe-paper);
  color: var(--cafe-dark);
}

.modal-body {
  padding: 20px 24px;
  overflow-y: auto;
  flex: 1;
}

.modal-footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 12px;
  padding: 16px 24px;
  border-top: 2px solid var(--cafe-paper);
}
```

```javascript
// JS para abrir/fechar modal
function openModal(modalId) {
  const modal = document.getElementById(modalId);
  modal.classList.remove('is-closing');
  modal.style.display = 'flex';
  document.body.style.overflow = 'hidden';
}

function closeModal(modalId) {
  const modal = document.getElementById(modalId);
  modal.classList.add('is-closing');
  setTimeout(() => {
    modal.style.display = 'none';
    modal.classList.remove('is-closing');
    document.body.style.overflow = '';
  }, 200);
}

// Fechar ao clicar no overlay
document.addEventListener('click', (e) => {
  if (e.target.classList.contains('modal-overlay')) {
    closeModal(e.target.id);
  }
});

// Fechar com ESC
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    const openOverlay = document.querySelector('.modal-overlay[style*="display: flex"]');
    if (openOverlay) closeModal(openOverlay.id);
  }
});
```

```html
<!-- Estrutura HTML -->
<div class="modal-overlay" id="modalExemplo" style="display:none;">
  <div class="modal-box">
    <div class="modal-header">
      <h2 class="modal-header__title">Título do Modal</h2>
      <button class="modal-header__close" onclick="closeModal('modalExemplo')">
        <i class="fa-solid fa-xmark"></i>
      </button>
    </div>
    <div class="modal-body">
      <!-- Conteúdo com scroll interno automático -->
    </div>
    <div class="modal-footer">
      <button class="btn btn--ghost" onclick="closeModal('modalExemplo')">Cancelar</button>
      <button class="btn btn--primary">Salvar</button>
    </div>
  </div>
</div>
```

---

## 🧭 SIDEBAR / NAVEGAÇÃO

```css
.sidebar {
  display: flex;
  flex-direction: column;
  width: 260px;
  min-height: 100vh;
  background: var(--cafe-dark);
  padding: 20px 0;
}

.sidebar__brand {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 20px 20px;
  border-bottom: 1px solid var(--cafe-medium);
  margin-bottom: 12px;
}

.sidebar__brand-icon {
  font-size: 24px;
  color: var(--cafe-caramel);
}

.sidebar__brand-name {
  font-family: var(--font-display);
  font-size: 18px;
  font-weight: 700;
  color: var(--cafe-cream);
}

.sidebar__nav {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding: 0 8px;
}

.sidebar__link {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 16px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  color: var(--cafe-gold);
  text-decoration: none;
  border-radius: var(--radius-sm);
  border-left: 3px solid transparent;
  transition: all 0.15s ease;
  cursor: pointer;
}

.sidebar__link:hover {
  background: var(--cafe-medium);
  color: var(--cafe-cream);
}

.sidebar__link.is-active {
  background: rgba(198,134,66,0.15);
  color: var(--cafe-cream);
  border-left-color: var(--cafe-caramel);
  font-weight: 600;
}

.sidebar__link-icon {
  width: 20px;
  text-align: center;
  font-size: 15px;
  flex-shrink: 0;
}

.sidebar__section-title {
  padding: 16px 20px 8px;
  font-size: 11px;
  font-weight: 600;
  color: var(--cafe-gold);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  opacity: 0.6;
}
```

```html
<aside class="sidebar">
  <div class="sidebar__brand">
    <i class="fa-solid fa-mug-hot sidebar__brand-icon"></i>
    <span class="sidebar__brand-name">Dashboard Café</span>
  </div>
  <span class="sidebar__section-title">Menu</span>
  <nav class="sidebar__nav">
    <a class="sidebar__link is-active" href="#reunioes">
      <i class="fa-solid fa-calendar sidebar__link-icon"></i> Reuniões
    </a>
    <a class="sidebar__link" href="#projetos">
      <i class="fa-solid fa-folder sidebar__link-icon"></i> Projetos
    </a>
    <a class="sidebar__link" href="#relatorios">
      <i class="fa-solid fa-chart-bar sidebar__link-icon"></i> Relatórios
    </a>
    <a class="sidebar__link" href="#config">
      <i class="fa-solid fa-gear sidebar__link-icon"></i> Configurações
    </a>
  </nav>
</aside>
```

---

## 💀 SKELETON LOADING

### Skeleton para Tabela

```css
.skeleton {
  background: linear-gradient(90deg, var(--cafe-paper) 25%, var(--cafe-beige) 50%, var(--cafe-paper) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
  border-radius: 4px;
}

.skeleton-table {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 20px;
}

.skeleton-table__header {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr 80px;
  gap: 12px;
  padding-bottom: 12px;
  border-bottom: 2px solid var(--cafe-paper);
}

.skeleton-table__header-cell {
  height: 14px;
  border-radius: 4px;
}

.skeleton-table__row {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr 80px;
  gap: 12px;
  padding: 12px 0;
  border-bottom: 1px solid var(--cafe-paper);
}

.skeleton-table__cell {
  height: 16px;
  border-radius: 4px;
}

.skeleton-table__cell--short {
  width: 60%;
}
```

### Skeleton para Cards

```css
.skeleton-card {
  background: var(--cafe-cream);
  border-radius: var(--radius-md);
  padding: 20px 24px;
  border: 1px solid var(--cafe-paper);
}

.skeleton-card__title {
  height: 20px;
  width: 65%;
  margin-bottom: 16px;
}

.skeleton-card__text {
  height: 14px;
  width: 100%;
  margin-bottom: 10px;
}

.skeleton-card__text--short {
  width: 45%;
}

.skeleton-card__value {
  height: 36px;
  width: 40%;
  margin-top: 8px;
}
```

```html
<!-- Skeleton de tabela -->
<div class="skeleton-table">
  <div class="skeleton-table__header">
    <div class="skeleton skeleton-table__header-cell"></div>
    <div class="skeleton skeleton-table__header-cell"></div>
    <div class="skeleton skeleton-table__header-cell"></div>
    <div class="skeleton skeleton-table__header-cell"></div>
  </div>
  <div class="skeleton-table__row">
    <div class="skeleton skeleton-table__cell"></div>
    <div class="skeleton skeleton-table__cell skeleton-table__cell--short"></div>
    <div class="skeleton skeleton-table__cell"></div>
    <div class="skeleton skeleton-table__cell skeleton-table__cell--short"></div>
  </div>
  <!-- Repetir 4-6 vezes para efeito visual -->
</div>

<!-- Skeleton de card KPI -->
<div class="skeleton-card">
  <div class="skeleton skeleton-card__title"></div>
  <div class="skeleton skeleton-card__text"></div>
  <div class="skeleton skeleton-card__value"></div>
</div>
```

---

## 🔔 TOAST NOTIFICATION

```css
.toast-container {
  position: fixed;
  bottom: 24px;
  right: 24px;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  gap: 8px;
  pointer-events: none;
}

.toast {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 20px;
  min-width: 280px;
  max-width: 420px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  color: var(--text-light);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
  animation: slideInToast 0.3s ease;
  pointer-events: auto;
}

.toast.is-leaving {
  animation: slideOutToast 0.3s ease forwards;
}

.toast--success {
  background: var(--cafe-caramel);
}

.toast--error {
  background: var(--status-late);
}

.toast--warning {
  background: var(--cafe-gold);
  color: var(--cafe-dark);
}

.toast--info {
  background: var(--cafe-medium);
}

.toast__icon {
  font-size: 16px;
  flex-shrink: 0;
}

.toast__message {
  flex: 1;
}

.toast__close {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: rgba(255,255,255,0.2);
  border: none;
  color: inherit;
  font-size: 11px;
  cursor: pointer;
  flex-shrink: 0;
  transition: background 0.15s ease;
}

.toast__close:hover {
  background: rgba(255,255,255,0.35);
}
```

```javascript
// Sistema de toast aprimorado com container
function initToastContainer() {
  if (!document.querySelector('.toast-container')) {
    const container = document.createElement('div');
    container.className = 'toast-container';
    document.body.appendChild(container);
  }
}

function showToast(message, type = 'success', duration = 3500) {
  initToastContainer();
  const container = document.querySelector('.toast-container');
  
  const iconMap = {
    success: 'fa-check-circle',
    error:   'fa-circle-exclamation',
    warning: 'fa-triangle-exclamation',
    info:    'fa-circle-info'
  };
  
  const toast = document.createElement('div');
  toast.className = `toast toast--${type}`;
  toast.innerHTML = `
    <i class="fa-solid ${iconMap[type]} toast__icon"></i>
    <span class="toast__message">${message}</span>
    <button class="toast__close" onclick="this.parentElement.remove()">
      <i class="fa-solid fa-xmark"></i>
    </button>
  `;
  
  container.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('is-leaving');
    setTimeout(() => toast.remove(), 300);
  }, duration);
}
```

---

## 📂 DROPDOWN / SELECT CUSTOMIZADO

```css
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown__trigger {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 14px;
  min-height: 38px;
  font-family: var(--font-body);
  font-size: 13px;
  font-weight: 500;
  color: var(--text-primary);
  background: var(--cafe-cream);
  border: 2px solid var(--cafe-paper);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all 0.15s ease;
}

.dropdown__trigger:hover {
  border-color: var(--cafe-gold);
}

.dropdown__trigger.is-open {
  border-color: var(--cafe-caramel);
  box-shadow: 0 0 0 3px rgba(198,134,66,0.1);
}

.dropdown__trigger-arrow {
  font-size: 10px;
  color: var(--text-secondary);
  transition: transform 0.2s ease;
}

.dropdown__trigger.is-open .dropdown__trigger-arrow {
  transform: rotate(180deg);
}

.dropdown__menu {
  position: absolute;
  top: calc(100% + 6px);
  left: 0;
  min-width: 180px;
  max-height: 240px;
  overflow-y: auto;
  background: var(--cafe-cream);
  border: 1px solid var(--cafe-paper);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-md);
  z-index: 100;
  animation: scaleIn 0.15s ease;
  display: none;
}

.dropdown__menu.is-open {
  display: block;
}

.dropdown__item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 14px;
  font-size: 13px;
  font-weight: 400;
  color: var(--text-primary);
  cursor: pointer;
  transition: background 0.1s ease;
  border: none;
  background: none;
  width: 100%;
  text-align: left;
  font-family: var(--font-body);
}

.dropdown__item:hover {
  background: var(--cafe-beige);
}

.dropdown__item.is-selected {
  background: var(--cafe-paper);
  font-weight: 600;
  color: var(--cafe-caramel);
}

.dropdown__divider {
  height: 1px;
  background: var(--cafe-paper);
  margin: 4px 0;
}
```

```javascript
// JS para dropdown
function toggleDropdown(triggerId) {
  const trigger = document.getElementById(triggerId);
  const menu = trigger.nextElementSibling;
  const isOpen = menu.classList.contains('is-open');
  
  // Fechar todos os outros
  document.querySelectorAll('.dropdown__menu.is-open').forEach(m => {
    m.classList.remove('is-open');
    m.previousElementSibling.classList.remove('is-open');
  });
  
  if (!isOpen) {
    trigger.classList.add('is-open');
    menu.classList.add('is-open');
  }
}

// Fechar ao clicar fora
document.addEventListener('click', (e) => {
  if (!e.target.closest('.dropdown')) {
    document.querySelectorAll('.dropdown__menu.is-open').forEach(m => {
      m.classList.remove('is-open');
      m.previousElementSibling.classList.remove('is-open');
    });
  }
});
```

---

## 🫗 EMPTY STATE

```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16px;
  padding: 60px 40px;
  text-align: center;
}

.empty-state__icon {
  font-size: 48px;
  color: var(--cafe-paper);
}

.empty-state__title {
  font-family: var(--font-display);
  font-size: 20px;
  font-weight: 600;
  color: var(--cafe-dark);
}

.empty-state__description {
  font-size: 14px;
  color: var(--text-secondary);
  max-width: 360px;
  line-height: 1.6;
}
```

```html
<div class="empty-state">
  <i class="fa-solid fa-mug-hot empty-state__icon"></i>
  <h3 class="empty-state__title">Nenhuma reunião encontrada</h3>
  <p class="empty-state__description">
    Parece que o café ainda não foi servido. Crie sua primeira reunião para começar.
  </p>
  <button class="btn btn--primary">
    <i class="fa-solid fa-plus"></i> Nova Reunião
  </button>
</div>
```

---

## 💬 TOOLTIP CSS PURO

```css
.tooltip {
  position: relative;
  display: inline-block;
}

.tooltip__text {
  position: absolute;
  bottom: calc(100% + 8px);
  left: 50%;
  transform: translateX(-50%) scale(0.9);
  padding: 6px 12px;
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 500;
  color: var(--text-light);
  background: var(--cafe-dark);
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: all 0.15s ease;
  z-index: 50;
}

.tooltip__text::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 5px solid transparent;
  border-top-color: var(--cafe-dark);
}

.tooltip:hover .tooltip__text {
  opacity: 1;
  transform: translateX(-50%) scale(1);
}

/* Variação: tooltip à direita */
.tooltip--right .tooltip__text {
  bottom: auto;
  left: calc(100% + 8px);
  top: 50%;
  transform: translateY(-50%) scale(0.9);
}

.tooltip--right .tooltip__text::after {
  top: 50%;
  left: auto;
  right: 100%;
  transform: translateY(-50%);
  border: 5px solid transparent;
  border-right-color: var(--cafe-dark);
}

.tooltip--right:hover .tooltip__text {
  transform: translateY(-50%) scale(1);
}
```

```html
<span class="tooltip">
  <button class="btn btn--ghost btn--icon" aria-label="Editar">
    <i class="fa-solid fa-pen"></i>
  </button>
  <span class="tooltip__text">Editar reunião</span>
</span>
```

---

## 📐 LAYOUTS COM CSS GRID

### Dashboard: Métricas + Tabela

```css
.layout-dashboard {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto 1fr;
  gap: 24px;
  padding: 24px;
  max-width: 1200px;
  margin: 0 auto;
}

.layout-dashboard__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 16px;
}

.layout-dashboard__metrics {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}

.layout-dashboard__content {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
```

```html
<div class="layout-dashboard">
  <div class="layout-dashboard__header">
    <h1>Dashboard de Reuniões</h1>
    <button class="btn btn--primary"><i class="fa-solid fa-plus"></i> Nova Reunião</button>
  </div>
  <div class="layout-dashboard__metrics">
    <!-- Cards KPI aqui -->
  </div>
  <div class="layout-dashboard__content">
    <!-- Tabela aqui -->
  </div>
</div>
```

### Formulário 2 Colunas Responsivo

```css
.layout-form {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px 24px;
  max-width: 700px;
}

.layout-form__full {
  grid-column: 1 / -1;
}

.layout-form__actions {
  grid-column: 1 / -1;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  padding-top: 12px;
  border-top: 1px solid var(--cafe-paper);
}

@media (max-width: 600px) {
  .layout-form {
    grid-template-columns: 1fr;
  }
}
```

### Sidebar Fixa + Conteúdo Principal

```css
.layout-app {
  display: grid;
  grid-template-columns: 260px 1fr;
  min-height: 100vh;
}

.layout-app__sidebar {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

.layout-app__main {
  padding: 24px 32px;
  background: var(--cafe-beige);
  overflow-y: auto;
  min-height: 100vh;
}

@media (max-width: 768px) {
  .layout-app {
    grid-template-columns: 1fr;
  }
  
  .layout-app__sidebar {
    position: fixed;
    left: -260px;
    z-index: 500;
    width: 260px;
    transition: left 0.3s ease;
  }
  
  .layout-app__sidebar.is-open {
    left: 0;
  }
  
  .layout-app__main {
    padding: 16px;
  }
}
```

---

## 📱 REGRAS DE RESPONSIVIDADE (IFRAME GAS)

### Breakpoints

```css
/* Mobile — dentro de sidebar ou iframe pequeno */
@media (max-width: 480px) {
  /* Formulários em coluna única */
  /* Tabelas com scroll horizontal */
  /* Cards empilhados */
  /* Botões full-width */
}

/* Tablet / iframe médio */
@media (max-width: 768px) {
  /* Grid de 2 colunas vira 1 */
  /* Sidebar colapsa */
  /* Modais quase full-screen */
}

/* Desktop padrão — 769px+ */
/* Layout completo com sidebar + grids de 3-4 colunas */
```

### Tabelas Responsivas no Iframe

```css
.table-scroll {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

/* Scrollbar customizada com tema café */
.table-scroll::-webkit-scrollbar {
  height: 6px;
}

.table-scroll::-webkit-scrollbar-track {
  background: var(--cafe-beige);
  border-radius: 3px;
}

.table-scroll::-webkit-scrollbar-thumb {
  background: var(--cafe-paper);
  border-radius: 3px;
}

.table-scroll::-webkit-scrollbar-thumb:hover {
  background: var(--cafe-gold);
}
```

### Scrollbar Global do Iframe

```css
body::-webkit-scrollbar {
  width: 8px;
}

body::-webkit-scrollbar-track {
  background: var(--cafe-beige);
}

body::-webkit-scrollbar-thumb {
  background: var(--cafe-paper);
  border-radius: 4px;
}

body::-webkit-scrollbar-thumb:hover {
  background: var(--cafe-gold);
}
```

### Lidando com Zoom do Browser

```css
/* Usar unidades relativas quando possível */
html {
  font-size: 100%; /* respeita zoom do browser */
}

/* Larguras com min/max para não quebrar com zoom */
.modal-box {
  width: min(600px, 95vw);
}

.table-container {
  max-width: 100%;
}

/* Inputs nunca menores que 44px (toque) */
.input, .select, .btn {
  min-height: 44px;
}
```

---

## ✨ 30 MICRO-INTERAÇÕES CSS PURO

### 1-5: Hover em Botões

```css
/* 1. Elevação suave */
.hover-lift:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

/* 2. Brilho sutil */
.hover-glow:hover {
  box-shadow: 0 0 0 4px rgba(198,134,66,0.15);
}

/* 3. Expansão de fundo */
.hover-fill {
  position: relative;
  overflow: hidden;
  z-index: 0;
}
.hover-fill::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--cafe-medium);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s ease;
  z-index: -1;
}
.hover-fill:hover::before {
  transform: scaleX(1);
}

/* 4. Compressão no click */
.hover-press:active {
  transform: scale(0.95);
  transition: transform 0.1s ease;
}

/* 5. Sublinhado animado */
.hover-underline {
  position: relative;
}
.hover-underline::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--cafe-caramel);
  transition: width 0.3s ease;
}
.hover-underline:hover::after {
  width: 100%;
}
```

### 6-10: Hover em Cards

```css
/* 6. Card com borda animada */
.card-hover-border {
  border: 2px solid transparent;
  transition: all 0.25s ease;
}
.card-hover-border:hover {
  border-color: var(--cafe-caramel);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

/* 7. Card com destaque lateral */
.card-hover-accent {
  border-left: 3px solid transparent;
  transition: all 0.25s ease;
}
.card-hover-accent:hover {
  border-left-color: var(--cafe-caramel);
  background: var(--cafe-beige);
}

/* 8. Card com ícone que cresce */
.card-hover-icon:hover .card__kpi-icon {
  transform: scale(1.2);
  color: var(--cafe-caramel);
  transition: all 0.3s ease;
}

/* 9. Card com sombra quente */
.card-hover-warm:hover {
  box-shadow: 0 8px 24px rgba(198,134,66,0.2);
}

/* 10. Card inteiro clicável */
.card-hover-click {
  cursor: pointer;
}
.card-hover-click:hover {
  background: var(--cafe-beige);
}
.card-hover-click:active {
  transform: scale(0.99);
}
```

### 11-15: Hover em Linhas de Tabela

```css
/* 11. Highlight suave */
.table__row:hover {
  background: var(--cafe-paper);
}

/* 12. Borda esquerda aparece */
.row-hover-accent {
  border-left: 3px solid transparent;
  transition: all 0.15s ease;
}
.row-hover-accent:hover {
  border-left-color: var(--cafe-caramel);
}

/* 13. Botões de ação revelados */
.row-hover-actions .table__cell--actions .btn {
  opacity: 0;
  transition: opacity 0.15s ease;
}
.row-hover-actions:hover .table__cell--actions .btn {
  opacity: 1;
}

/* 14. Texto fica mais escuro */
.row-hover-dark:hover .table__cell {
  color: var(--cafe-dark);
}

/* 15. Fundo desliza da esquerda */
.row-hover-slide {
  position: relative;
  z-index: 0;
}
.row-hover-slide::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--cafe-paper);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.2s ease;
  z-index: -1;
}
.row-hover-slide:hover::before {
  transform: scaleX(1);
}
```

### 16-20: Transições de Modal e Toast

```css
/* 16. Modal fade + slide */
@keyframes modalEnter {
  from { opacity: 0; transform: translateY(20px) scale(0.98); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

/* 17. Modal sai suavemente */
@keyframes modalExit {
  from { opacity: 1; transform: scale(1); }
  to   { opacity: 0; transform: scale(0.96); }
}

/* 18. Toast entra pela direita */
@keyframes toastSlideIn {
  from { opacity: 0; transform: translateX(100px); }
  to   { opacity: 1; transform: translateX(0); }
}

/* 19. Toast sai com fade + compressão */
@keyframes toastSlideOut {
  from { opacity: 1; transform: translateX(0); max-height: 100px; }
  to   { opacity: 0; transform: translateX(60px); max-height: 0; padding: 0; margin: 0; }
}

/* 20. Overlay pulsa suavemente no foco */
@keyframes overlayPulse {
  0%, 100% { background: rgba(44,26,14,0.6); }
  50%      { background: rgba(44,26,14,0.55); }
}
```

### 21-25: Animações de Status

```css
/* 21. Spinner de loading circular */
.spinner {
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 2px solid var(--cafe-paper);
  border-top-color: var(--cafe-caramel);
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

.spinner--large {
  width: 40px;
  height: 40px;
  border-width: 3px;
}

/* 22. Pulse em alertas/badges */
.pulse-alert {
  animation: pulseGlow 2s ease-in-out infinite;
}
@keyframes pulseGlow {
  0%, 100% { box-shadow: 0 0 0 0 rgba(139,58,42,0.3); }
  50%      { box-shadow: 0 0 0 8px rgba(139,58,42,0); }
}

/* 23. Dot pulsante (indicador de ativo) */
.dot-pulse {
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--status-done);
  animation: dotPulse 1.5s ease-in-out infinite;
}
@keyframes dotPulse {
  0%, 100% { transform: scale(1); opacity: 1; }
  50%      { transform: scale(1.5); opacity: 0.6; }
}

/* 24. Contagem animada (número que cresce) */
.count-up {
  animation: countFadeIn 0.5s ease;
}
@keyframes countFadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* 25. Check animado (para sucesso) */
.check-animated {
  display: inline-block;
  animation: bounceIn 0.5s ease;
  color: var(--status-done);
}
```

### 26-30: Shimmer e Efeitos Especiais

```css
/* 26. Shimmer para skeleton loading */
.shimmer {
  background: linear-gradient(
    90deg,
    var(--cafe-paper) 25%,
    var(--cafe-beige) 50%,
    var(--cafe-paper) 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
}

/* 27. Barra de progresso animada */
.progress-bar {
  height: 6px;
  background: var(--cafe-paper);
  border-radius: 3px;
  overflow: hidden;
}
.progress-bar__fill {
  height: 100%;
  background: var(--cafe-caramel);
  border-radius: 3px;
  transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

/* 28. Fade in escalonado (para listas) */
.stagger-in > * {
  opacity: 0;
  animation: slideUp 0.3s ease forwards;
}
.stagger-in > *:nth-child(1) { animation-delay: 0.05s; }
.stagger-in > *:nth-child(2) { animation-delay: 0.10s; }
.stagger-in > *:nth-child(3) { animation-delay: 0.15s; }
.stagger-in > *:nth-child(4) { animation-delay: 0.20s; }
.stagger-in > *:nth-child(5) { animation-delay: 0.25s; }
.stagger-in > *:nth-child(6) { animation-delay: 0.30s; }
.stagger-in > *:nth-child(7) { animation-delay: 0.35s; }
.stagger-in > *:nth-child(8) { animation-delay: 0.40s; }

/* 29. Ripple effect no click (CSS puro com :active) */
.ripple {
  position: relative;
  overflow: hidden;
}
.ripple::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at var(--x, 50%) var(--y, 50%), rgba(255,255,255,0.3) 0%, transparent 60%);
  opacity: 0;
  transition: opacity 0.3s ease;
}
.ripple:active::after {
  opacity: 1;
}

/* 30. Ícone do café com vapor (decorativo) */
.coffee-steam {
  position: relative;
  display: inline-block;
}
.coffee-steam::before,
.coffee-steam::after {
  content: '';
  position: absolute;
  bottom: 100%;
  width: 2px;
  height: 12px;
  background: var(--cafe-gold);
  border-radius: 2px;
  opacity: 0.4;
  animation: steam 2s ease-in-out infinite;
}
.coffee-steam::before {
  left: 35%;
  animation-delay: 0.3s;
}
.coffee-steam::after {
  left: 60%;
  animation-delay: 0.8s;
}
@keyframes steam {
  0%   { transform: translateY(0) scaleY(1); opacity: 0.4; }
  50%  { transform: translateY(-8px) scaleY(1.2); opacity: 0.2; }
  100% { transform: translateY(-16px) scaleY(0.8); opacity: 0; }
}
```

---

## 🎯 REFERÊNCIA RÁPIDA DE CLASSES

| Componente | Classe principal | Modificadores |
|---|---|---|
| Botão | `.btn` | `--primary`, `--secondary`, `--ghost`, `--danger`, `--icon`, `.is-loading` |
| Input | `.input` | `--error`, `--success`, `:disabled` |
| Select | `.select` | (dentro de `.select-wrapper`) |
| Textarea | `.textarea` | (mesmos estados do input) |
| Checkbox | `.checkbox` | `.checkbox__input`, `.checkbox__mark` |
| Radio | `.radio` | `.radio__input`, `.radio__mark` |
| Tabela | `.table` | `.table__head`, `.table__row`, `.table__cell` |
| Card | `.card` | `--header`, `--kpi` |
| Badge | `.badge` | `--done`, `--progress`, `--late`, `--pending` |
| Modal | `.modal-overlay` | `.modal-box`, `--wide`, `--narrow`, `.is-closing` |
| Toast | `.toast` | `--success`, `--error`, `--warning`, `--info`, `.is-leaving` |
| Sidebar | `.sidebar` | `.sidebar__link`, `.is-active` |
| Dropdown | `.dropdown` | `.dropdown__menu`, `.is-open` |
| Empty | `.empty-state` | — |
| Tooltip | `.tooltip` | `--right` |
| Skeleton | `.skeleton` | `.skeleton-table`, `.skeleton-card` |
| Spinner | `.spinner` | `--large` |
| Layout | `.layout-dashboard` | `.layout-form`, `.layout-app` |
| Form | `.form` | `.form__row`, `--full`, `.form-field` |

---

*Referência de componentes CSS — Tema Café ☕ — Projeto Dashboard GAS*
