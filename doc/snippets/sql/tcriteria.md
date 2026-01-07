# 🧠 TCriteria – Dicas de Uso  
(MadBuilder / Adianti)

Este snippet reúne **dicas práticas e exemplos reais de uso da `TCriteria`**,
utilizada para montar **consultas dinâmicas**, filtros compostos e condições
avançadas no **Adianti / MadBuilder**.

A `TCriteria` é essencial quando:
- há necessidade de `AND` e `OR` combinados
- filtros são opcionais
- consultas dependem do usuário logado
- filtros vêm de formulários

---

## 📌 O que é a TCriteria?

A `TCriteria` representa um **conjunto de filtros** (`TFilter`) que podem ser
combinados com operadores lógicos.

Ela é usada em:
- `load()`
- `count()`
- `delete()`
- `TRepository`
- filtros de listagens

---

## 🧱 Estrutura básica

```php
$criteria = new TCriteria;
$criteria->add(new TFilter('status', '=', 'A'));

$registros = Pedido::getObjects($criteria);
```

---

## ➕ Adicionando múltiplos filtros (AND)

Por padrão, os filtros usam **AND**.

```php
$criteria = new TCriteria;
$criteria->add(new TFilter('ativo', '=', 'S'));
$criteria->add(new TFilter('valor', '>', 0));
```

Resultado lógico:
```
ativo = 'S' AND valor > 0
```

---

## 🔀 Utilizando operador OR

```php
$criteria = new TCriteria;
$criteria->add(new TFilter('status', '=', 'A'), TExpression::OR_OPERATOR);
$criteria->add(new TFilter('status', '=', 'P'), TExpression::OR_OPERATOR);
```

Resultado:
```
status = 'A' OR status = 'P'
```

---

## 🧩 Combinando AND e OR (Critério composto)

Quando precisar misturar operadores, **crie critérios internos**.

### Exemplo:
Filtrar registros do usuário **OU** dos responsáveis

```php
$user_id = TSession::getValue('userid');

$criteria_pessoas = new TCriteria;
$criteria_pessoas->add(
    new TFilter(
        'pessoas_id',
        '=',
        "(SELECT id FROM pessoas WHERE system_users_id = '{$user_id}')"
    ),
    TExpression::OR_OPERATOR
);

$criteria_pessoas->add(
    new TFilter(
        'pessoas_id',
        'IN',
        "(SELECT id FROM pessoas WHERE responsavel_financeiro_id 
          IN (SELECT id FROM pessoas WHERE system_users_id = '{$user_id}'))"
    ),
    TExpression::OR_OPERATOR
);

// Critério principal
$this->filter_criteria->add($criteria_pessoas, TExpression::OR_OPERATOR);
```

---

## 📥 Filtro IN

```php
$criteria = new TCriteria;
$criteria->add(new TFilter('id', 'IN', $ids));
```

📌 `$ids` pode ser:
- array
- string SQL
- subquery

---

## 🚫 Filtro IS NULL / IS NOT NULL

```php
$criteria->add(new TFilter('data_email', 'IS NOT', NULL));
```

```php
$criteria->add(new TFilter('data_exclusao', 'IS', NULL));
```

---

## 🔢 Contagem com TCriteria

```php
$criteria = new TCriteria;
$criteria->add(new TFilter('turma_id', '=', $turmaId));

$total = Matricula::count($criteria);
```

---

## 📊 Ordenação, limite e offset

```php
$criteria = new TCriteria;
$criteria->setProperty('order', 'id DESC');
$criteria->setProperty('limit', 10);
$criteria->setProperty('offset', 0);
```

---

## 🧠 TCriteria com filtros opcionais

Muito comum em formulários de pesquisa:

```php
$criteria = new TCriteria;

if (!empty($data->status)) {
    $criteria->add(new TFilter('status', '=', $data->status));
}

if (!empty($data->data_inicio)) {
    $criteria->add(new TFilter('data', '>=', $data->data_inicio));
}
```

---

## 🧮 GROUP BY com TCriteria

A `TCriteria` também permite aplicar **GROUP BY** através das propriedades
internas da consulta.

Isso é feito utilizando `setProperty()`.

---

### 📌 Exemplo básico – GROUP BY simples

```php
$criteria = new TCriteria;
$criteria->setProperty('group', 'turma_id');

$dados = Matricula::getObjects($criteria);
```

Resultado SQL aproximado:
```sql
GROUP BY turma_id
```

---

### 📊 GROUP BY com COUNT

Muito comum para relatórios e dashboards:

```php
$criteria = new TCriteria;
$criteria->setProperty('group', 'status');

$registros = Matricula::select('status', 'COUNT(*) as total')
    ->where('ativo', '=', 'S')
    ->load($criteria);
```

📌 Cada linha retornada conterá:
- `status`
- `total`

---

### 🔗 GROUP BY com múltiplas colunas

```php
$criteria = new TCriteria;
$criteria->setProperty('group', 'curso_id, status');

$dados = Matricula::getObjects($criteria);
```

Resultado:
```sql
GROUP BY curso_id, status
```

---

### 🧠 GROUP BY + ORDER BY

```php
$criteria = new TCriteria;
$criteria->setProperty('group', 'status');
$criteria->setProperty('order', 'total DESC');

$dados = Matricula::select('status', 'COUNT(*) as total')
    ->load($criteria);
```

---

### 🧩 GROUP BY + HAVING

O `HAVING` pode ser usado com `TFilter` normalmente:

```php
$criteria = new TCriteria;
$criteria->setProperty('group', 'turma_id');

$criteria->add(new TFilter('COUNT(*)', '>', 10));

$dados = Matricula::select('turma_id', 'COUNT(*) as total')
    ->load($criteria);
```

📌 Importante:
- O campo do `TFilter` deve ser a **expressão SQL**
- Funciona apenas em consultas agrupadas

---

## ⚠️ Pontos de atenção

- `group` é definido via `setProperty`
- Sempre combine `GROUP BY` com `select()` quando usar agregações
- Evite `select *` com agrupamentos
- Use aliases (`as total`) para facilitar leitura

---

## 💡 Boas práticas com GROUP BY

- Use apenas colunas necessárias no `select`
- Prefira métodos específicos para relatórios
- Combine com `order` para dashboards
- Centralize consultas agrupadas em métodos do model


---

## ⚠️ Erros comuns

❌ Misturar AND e OR sem critérios internos  
❌ Concatenar SQL manualmente  
❌ Não validar valores vindos do formulário  
❌ Criar filtros dentro de loops sem necessidade

---

## 💡 Boas práticas

- Use `TCriteria` para consultas complexas
- Prefira `TFilter` ao invés de SQL manual
- Separe critérios compostos
- Centralize filtros reutilizáveis em métodos
- Evite lógica pesada dentro de controllers

---

## 📎 Observação final

Este snippet cobre **uso prático da TCriteria**.

Para consultas mais simples, veja:
- `banco-de-dados.md`

Para filtros em listagens:
- `listagens-e-kanban.md`
