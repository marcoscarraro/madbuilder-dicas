# 🎨 Guideline de UI/UX – Padrões Gerais do Projeto  
**MadBuilder / Adianti**

Este documento define os **padrões oficiais de UI/UX** adotados nos projetos, com foco em **consistência**, **previsibilidade** e **alta usabilidade**.

---

## 1️⃣ Padrões de LIST (Listagens)

### 1.1 Estrutura da Listagem

- **Título:** Nome do módulo no plural  
  Ex.: Produtos, Pessoas, Planos
- **Paginação:** Ativada
- **Manter paginação após ações:** Sim
- **Habilitar limites de colunas e linhas:** Sim
- **Colunas:** Todas ordenáveis
- **Busca global:** No cabeçalho
- **Filtros:** Em cortina lateral (80%)
- **Clique padrão na linha:** Desabilitado

### 1.2 Coluna de Ações

- **Posicionamento:** À direita da tabela
- **Ações:** Agrupadas em menu

### 1.3 Cabeçalho da LIST

**Botão Cadastrar**
- Cor: Verde

**Busca Global**
- Largura: 220px
- Ícone: `fas fa-search`

**Ordem visual**
1. Cadastrar
2. Atualizar
3. Campo de busca
4. Buscar
5. Limpar filtros

---

## 2️⃣ Filtros da LIST

- Abertura em **cortina lateral**
- Largura: 80%
- Título: `Filtros de [Módulo]`  
  Ex.: Filtros de Planos, Filtros de Alunos
- Botão **Buscar:** Estilo Default / Branco

---

## 3️⃣ Padrões de FORM (Cadastro / Edição)

### 3.1 Estrutura

- **Título:** `Cadastro de [Módulo]`  
  Ex.: Cadastro de Plano, Cadastro de Aluno
- Campo **ID:** Hidden
- **Breadcrumb:** Removido
- Abertura em **cortina lateral (80%)**
- Fechar cortina após salvar: **Sim**

### 3.2 Ações do Formulário

- **Botão Salvar:** Verde
- **Demais botões:** Default / Branco

---

## 4️⃣ Elementos Globais

- Tipo de diálogo: **Sweet Alert**
- Toasts de feedback: **Ativados**
- Font Awesome incluído globalmente
```html
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css"
  integrity="sha512-Evv84Mr4kqVGRNSgIGL/F/aIDqQb7xQ2vcrdIwxfjThSH8CSR7PBEakCr51Ck+w+/U6swU2Im1vVX0SVk9ABhg=="
  crossorigin="anonymous"
  referrerpolicy="no-referrer"
/>
´´´html
---

## 5️⃣ Padrão – Gerador de CRUD

- Formulário com **rótulo acima do input**
- **Sem uso de colunas** no formulário
- Cadastro em **cortina lateral**
- Listagem com **busca**
- Toast de feedback: **Ativado**

---

## 6️⃣ Regras Gerais de Navegação

### LIST e FORM

- **Sem ícone no menu lateral**
