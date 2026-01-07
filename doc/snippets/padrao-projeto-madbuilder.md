# 🧱 Padrão de Projeto – MadBuilder  

Este documento descreve o **padrão de configuração, organização e boas práticas**
utilizado nos projetos desenvolvidos com **MadBuilder / Adianti**.

O objetivo é:
- manter consistência entre projetos
- acelerar desenvolvimento
- facilitar manutenção
- garantir boa experiência de uso (UX)

---

## ⚙️ Configurações iniciais do projeto

### Propriedades do Projeto
- **Tipo da caixa de diálogo:** `Sweet Alert`

Isso garante:
- alertas mais modernos
- confirmação visual clara
- melhor UX em ações críticas

---

## 🧩 Header Tags globais

Adicionar nas **Header Tags** do projeto:

```html
<!-- Font Awesome -->
<link rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css"
      integrity="sha512-Evv84Mr4kqVGRNSgIGL/F/aIDqQb7xQ2vcrdIwxfjThSH8CSR7PBEakCr51Ck+w+/U6swU2Im1vVX0SVk9ABhg=="
      crossorigin="anonymous"
      referrerpolicy="no-referrer" />
```

📌 Utilizado para:
- ícones de busca
- botões de ação
- reforço visual em listagens e formulários

---

## 🧾 Padrão de CRUD

### Formulário
- Rótulo **acima** do input
- **Sem colunas** (layout fluido)
- Campo `ID` sempre **hidden**

### Cadastro
- Abertura em **cortina lateral**
- Largura padrão definida no form

### Listagem
- Busca no **cabeçalho**
- Ações com **Toast**
- Layout limpo e funcional

---

## 📋 HeaderList – Configurações padrão

### Propriedades do Form
- Paginação: **SIM**
- Manter paginação: **SIM**
- Habilitar limites de colunas e linhas
- Habilitar popover dos filtros de busca
- Desabilitar clique padrão da linha

### Busca global
- Campo de busca global habilitado
- Largura: **220px**
- Ícone: `fas fa-search`

### Ações padrão
Utilizar ações prontas:
- Atualizar
- Limpar filtros
- Buscar

### Organização visual dos elementos
Ordem recomendada:
1. Cadastrar
2. Atualizar
3. Input de busca
4. Buscar
5. Limpar

### Colunas
- **Todas as colunas ordenáveis**

### Página
- Remover ícone do menu lateral (quando aplicável)

---

## 🪟 Formulário de Cadastro (Cortina)

### Propriedades da página
- **Largura da cortina:** `80%`

### Campos
- Campo `ID` sempre como `hidden`
- Campos principais visíveis
- Campos técnicos ocultos

---

## 🗄️ Padrão de conexão com o banco de dados

### Estrutura obrigatória

```php
try {

    TTransaction::open(MAIN_DATABASE);

    // regras de negócio
    // persistência
    // chamadas de service

    TTransaction::close();

} catch (Exception $e) {

    TTransaction::rollback();
    throw $e;
}
```

📌 Regras importantes:
- Sempre abrir e fechar a transação
- Sempre usar `rollback` em caso de erro
- Nunca engolir exceções

---

## 🧠 Padrão para Classes Service

### O que a Service **PODE** fazer
- Requisições externas
- Tratamento de dados
- Validações de regra de negócio
- Processamento de informações

### O que a Service **NÃO PODE** fazer
- ❌ Abrir conexão com o banco
- ❌ Fechar transação
- ❌ Controlar `TTransaction`

📌 **Quem abre a conexão é sempre quem chama a Service.**

---

## ⚠️ Tratamento de erros na Service

### Padrão obrigatório
Sempre lançar exceções:

```php
throw new Exception('As senhas informadas não conferem!');
```

📌 Benefícios:
- Interrompe execução imediatamente
- Permite exibição da mensagem ao usuário
- Mantém controle no fluxo do sistema
- Integra perfeitamente com Sweet Alert

---

## ✅ Benefícios deste padrão

- Código previsível
- Fácil leitura
- Menos bugs em produção
- Melhor experiência do usuário
- Padronização entre equipes e projetos

---

## 📎 Observação final

Este padrão é **flexível**, mas deve ser seguido como base.

Ajustes são permitidos quando:
- há necessidade técnica
- há ganho claro de UX
- o padrão se mantém consistente

Documente qualquer exceção ao padrão.
