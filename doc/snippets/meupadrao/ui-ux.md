# 🎨 Padrão UI/UX – MadBuilder / Adianti

Este documento define as **regras visuais e comportamentais obrigatórias**
para garantir **consistência, clareza e usabilidade**
em sistemas desenvolvidos com **MadBuilder / Adianti**.

---

## 🎯 Objetivos do Padrão

- Padronização visual entre projetos
- Experiência consistente para o usuário
- Redução de erros e retrabalho
- Aumento de produtividade
- Facilidade de manutenção e onboarding de desenvolvedores

---

## 🪟 Navegação e Abertura de Telas

### 📋 Formulários (Ação)

#### ✅ Regra obrigatória
- **Todos os formulários devem abrir em cortina lateral**
- **Largura padrão:** `80%` da tela

#### Exemplos
- Cadastro  
- Edição  
- Configurações  
- Pré-matrícula  
- Contratos editáveis  

#### 🎯 Por que funciona bem
- Mantém o contexto da tela anterior
- Evita quebra de fluxo
- Dá espaço para formulários médios e grandes
- Permite agrupamento de campos, abas e seções

📌 **Padrão maduro**, utilizado em CRMs, ERPs e sistemas SaaS modernos.

⚠️ **Nunca abrir formulários em modal pequeno ou tela cheia**, salvo justificativa técnica.

---

### 📄 Documentos e Relatórios (Consulta)

#### ✅ Regra obrigatória
- **Documentos e relatórios devem abrir em modal**
- **Largura padrão:** `90%` da tela

#### Exemplos
- PDF  
- Relatórios  
- Contratos para leitura  
- Impressão e pré-visualização  

#### 🎯 Por que não usar cortina
- Documento não é ação, é consulta
- Usuário precisa de foco e leitura confortável
- Evita confusão com formulários

📌 Modal grande transmite claramente:
> “Estou apenas visualizando algo”

⚠️ **Nunca usar cortina para documentos ou relatórios.**

---

## 🧠 Regra de Ouro de UX

> **Ação → Cortina (80%)**  
> **Consulta → Modal (90%)**

Jamais misturar formulário com visualização de documento.

---

## 🔘 Padrão de Botões

### 🟢 Botão Primário (Ação Principal)

Usar **sempre** para ações finais e críticas:
- Salvar
- Cadastrar
- Confirmar
- Concluir

#### Regras
- Cor: **Verde**
- Classe padrão: `btn-success`
- **Apenas um botão primário por tela**
- Deve ter maior destaque visual

📌 Objetivo: deixar claro para o usuário **qual é a ação principal**.

---

### ⚪ Botões Secundários (Apoio / Navegação)

Usar para ações auxiliares:
- Pesquisar
- Listar
- Avançar
- Voltar
- Atualizar
- Limpar filtros

#### Regras
- Cor: Branco / Default
- Classe: `btn-default` ou `btn-secondary`
- Nunca competir visualmente com o botão primário

---

### 🔴 Botões de Risco

Usados para ações destrutivas:
- Excluir
- Cancelar definitivamente

#### Regras
- Cor: **Vermelho**
- **Sempre com confirmação** (Sweet Alert)

---

## 🧾 Padrão de Campos (Inputs)

### 📅 Campos de Data
- Largura fixa: `220px`
- Evita campos desproporcionais
- Melhora leitura e escaneabilidade

📌 Usar em:
- Data inicial / final
- Vencimentos
- Períodos

---

### 🔢 Campos Numéricos e Monetários

#### Valores monetários
- Sempre alinhados à direita
- Com máscara de moeda

#### Quantidades
- Alinhamento à direita

📌 Facilita comparação visual e leitura correta dos valores.

---

## 📊 Padrão para Tabelas de Listagem

### Colunas
- **Todas as colunas devem ser ordenáveis**
- Colunas de valor:
  - Alinhadas à direita
- Colunas de status:
  - Preferencialmente com badge visual

---

### 📐 Formatação obrigatória de dados

Nunca exibir dados “crus” ao usuário:

- 📞 Telefone → `(99) 99999-9999`
- 🆔 CPF → `999.999.999-99`
- 🏢 CNPJ → `99.999.999/0001-99`
- 💰 Monetário → `R$ 1.234,56`

📌 **Regra de ouro:**  
Se o dado é exibido ao usuário, ele **precisa estar legível e formatado**.

---

## 🔍 Padrão de Busca

### Busca global
- Largura: `220px`
- Ícone: `fas fa-search`
- Posição:
  - Próxima aos botões de ação
  - Nunca isolada ou escondida

### Filtros
- Utilizar popover de filtros
- Evitar excesso de campos visíveis
- Filtros avançados ficam no popover

---

## 📐 Organização Visual de Ações (HeaderList)

### Ordem padrão
1. Cadastrar (botão primário)
2. Atualizar
3. Campo de busca
4. Buscar
5. Limpar filtros

📌 Mantém fluxo mental previsível para o usuário.

---

## 🧠 Princípios UX Obrigatórios

- Não sobrecarregar telas
- Uma ação principal por tela
- Feedback visual sempre que possível
- Evitar campos técnicos visíveis
- Priorizar leitura e escaneabilidade

---

## 🚫 O que NÃO é permitido

- Botão “Salvar” que não seja verde
- Valores monetários alinhados à esquerda
- CPF, CNPJ ou telefone sem máscara
- Tabelas sem ordenação
- Campos de data com largura variável
- Telas sem hierarquia visual clara

---

## ✅ Resultado Esperado

- Interface previsível
- Menos erros do usuário
- Menos chamados de suporte
- Maior produtividade
- Experiência consistente entre sistemas

---
