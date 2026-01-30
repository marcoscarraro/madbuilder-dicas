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
- Atualizar
- Limpar filtros
- Buscar

### Organização visual
1. Cadastrar  
2. Atualizar  
3. Input de busca  
4. Buscar  
5. Limpar  

### Colunas
- **Todas ordenáveis**

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

📌 Regras:
- Sempre abrir e fechar a transação
- Sempre usar `rollback` em erro
- Nunca engolir exceções

---

## 🧠 Padrão para Classes Service

### A Service PODE
- Tratar dados
- Validar regras de negócio
- Fazer integrações externas

### A Service NÃO PODE
- ❌ Abrir ou fechar transação
- ❌ Controlar `TTransaction`

📌 Quem abre a conexão é **sempre quem chama** a Service.

---

## ⚠️ Tratamento de erros na Service

```php
throw new Exception('As senhas informadas não conferem!');
```

Benefícios:
- Interrompe execução
- Exibe mensagem ao usuário
- Integra com Sweet Alert

---

## 🚀 Configurações recomendadas do PHP (Produção)

Aplicar no `php.ini` (ou `.user.ini` / `php-fpm.conf`):

```ini
display_errors = Off
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT & ~E_NOTICE

session.use_only_cookies = 1
session.cookie_httponly = true
session.use_trans_sid = 0

session.entropy_file = /dev/urandom
session.entropy_length = 32
session.gc_maxlifetime = 14000
```

---

## 🧩 Padrão para valores fixos em tabelas (Status, Tipos, Flags)

Sempre que existir valores fixos (status, situações, tipos), **criar constantes no Model**.

### Exemplo

```php
class Pedido extends TRecord
{
    const STATUS_ATIVO    = 1;
    const STATUS_INATIVO  = 2;
    const STATUS_PENDENTE = 3;
    const STATUS_FATURADO = 4;
}
```

### Uso correto

```php
if ($pedido->status_id == Pedido::STATUS_FATURADO) {
    // lógica específica
}
```

---

## 🔁 Padrão de nomenclatura para eventos `onChange`

Sempre usar o padrão:

```
onChange[nomeDoCampo][Acao]
```

### Exemplos

```php
onChangeTipoPagamentoAtualizaParcelas
onChangeCidadeCarregaBairros
onChangeCursoAtualizaTurmas
```

---

## ✅ Benefícios deste padrão

- Código previsível
- Padronização entre projetos
- Menos bugs em produção
- Melhor UX
- Facilidade de manutenção

---

## 📎 Observação final

Este padrão deve ser usado como **base obrigatória**.

Exceções só são aceitas quando:
- houver necessidade técnica clara
- houver ganho real de UX
- a decisão for documentada

Padrão seguido = projeto saudável.
