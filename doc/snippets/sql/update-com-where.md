## 🧩 UPDATE com WHERE – Adianti / MadBuilder

O Adianti oferece **duas formas oficiais** de realizar `UPDATE` com cláusula `WHERE`,
sem a necessidade de carregar registros em memória.

Essas abordagens são **performáticas**, **seguras** e seguem o padrão do framework.

---

## 🥇 Forma 1 — UPDATE fluente via Active Record

Utiliza o Active Record de forma estática, com encadeamento de métodos.

### Exemplo

```php
try
{
    TTransaction::open('exemplos');

    Cliente::where('id', '<', 172)
           ->where('sexo', 'is', null)
           ->set('sexo', 'F')
           ->update();

    TTransaction::close();
}
catch (Exception $e)
{
    TTransaction::rollback();
    new TMessage('error', $e->getMessage());
}
```

### Características

- Não instancia objetos
- Executa `UPDATE` direto no banco
- Sintaxe curta e legível
- Permite múltiplos `where()`

### Quando usar

- Atualizações simples
- Flags, status, campos únicos
- Regras diretas e pouco dinâmicas

### Atenções

- Pouca flexibilidade para filtros condicionais
- Código pode ficar confuso com muitos `where()`
- Não recomendado para regras complexas

---

## 🥈 Forma 2 — UPDATE com TRepository + TCriteria

Abordagem mais flexível e recomendada para projetos maiores.

### Exemplo

```php
try
{
    TTransaction::open('exemplos');

    // Cria o repositório
    $repos = new TRepository('Cliente');

    // Cria os critérios
    $criteria = new TCriteria;
    $criteria->add(new TFilter('id', '<', 172));
    $criteria->add(new TFilter('sexo', 'is', null));

    // Define os valores de atualização
    $values = [
        'sexo' => 'F'
    ];

    $repos->update($values, $criteria);

    TTransaction::close();
}
catch (Exception $e)
{
    TTransaction::rollback();
    new TMessage('error', $e->getMessage());
}
```

### Características

- Critérios isolados em `TCriteria`
- Valores separados em array
- Fácil de estender e reutilizar
- Ideal para regras complexas

### Quando usar

- Filtros dinâmicos
- Atualizações em massa
- Código mais organizado
- Padrão enterprise

---

## 🚫 O que evitar

- ❌ Loop com `load()` + `save()` para UPDATE em massa
- ❌ SQL manual sem necessidade
- ❌ UPDATE sem critério (`WHERE`)

---

## 📎 Observação

Ambas as abordagens:
- Não carregam registros em memória
- Executam `UPDATE` direto no banco
- Seguem o padrão oficial do Adianti
