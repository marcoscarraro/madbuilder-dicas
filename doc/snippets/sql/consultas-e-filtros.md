# 🗃️ Consultas e Filtros – SQL (MadBuilder / Adianti)

Este arquivo reúne snippets para **consultas ao banco de dados**
utilizando o **Active Record do Adianti / MadBuilder**.

O foco aqui é:
- SELECTs
- filtros
- contagens
- relacionamentos
- boas práticas de leitura de dados

---

## 🔍 Consultas simples

### Buscar registros com `where`
```php
Curso::where('ativo', '=', 'S')->load();
```

---

## 🧩 Selecionar colunas específicas

Utilize `select()` para reduzir carga e melhorar performance.

```php
Calendario::select('data', 'ano')
    ->where('data_calendario', '=', $dataAula->format('Y-m-d'))
    ->load();
```

---

## 📊 Métodos auxiliares de consulta

```php
->load()        // Retorna todos os registros
->first()       // Retorna o primeiro registro
->last()        // Retorna o último registro
->orderBy('id') // Ordena por coluna
```

---

## 🔢 Contagem de registros

```php
$total = Matricula::where('turma_id', '=', $turmaId)->count();
```

---

## 🧠 Consultas condicionais

### Múltiplas condições
```php
Pedido::where('status', '=', 'A')
    ->where('valor_total', '>', 0)
    ->load();
```

---

## 🔗 Relacionamento entre tabelas

### Acessar dados relacionados
```php
echo $pedido->cliente->nome;
```

⚠️ Funciona apenas se a **FK estiver corretamente definida**
no modelo Active Record.

---

## 🛑 Tratamento de erros em consultas

```php
try {
    TTransaction::open(self::$database);

    $dados = Registro::where('status', '=', 'A')->load();

    TTransaction::close();
} catch (Exception $e) {
    TTransaction::rollback();
    throw $e;
}
```

---

## ⚠️ Boas práticas

- Prefira `select()` quando não precisar de todas as colunas
- Evite consultas dentro de loops
- Utilize `count()` ao invés de `load()` para contagens
- Sempre trate exceções com `try/catch`

---

## 📎 Observação final

Este arquivo cobre **consultas e filtros SQL**.  
Veja também:
- `consultas-avancadas.md`
- `banco-de-dados.md`
