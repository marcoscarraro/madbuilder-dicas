# 🗃️ Consultas e Log – SQL (MadBuilder / Adianti)

Este arquivo reúne snippets para **consultas ao banco de dados**,
**monitoramento de SQL**, **contagem de registros** e boas práticas
utilizando o **Adianti / MadBuilder**.

Os exemplos abaixo focam em leitura, análise e depuração de consultas.

---

## 🧾 Log de SQL

### Exibir SQL executado na tela
> Deve ser utilizado **apenas em desenvolvimento**
```php
TTransaction::setLogger(new TLoggerSTD());
```

---

## 🔍 Consultas simples

### Buscar registros com `where`
```php
Curso::where('ativo', '=', 'S')->load();
```

---

## 🧩 Selecionar colunas específicas

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

### Consulta com múltiplas condições
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

⚠️ Funciona apenas se a FK estiver corretamente definida.

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

- Use logs SQL somente em desenvolvimento
- Evite consultas dentro de loops
- Prefira métodos do Active Record
- Sempre trate exceções com `try/catch`

---

## 📎 Observação final

Este arquivo cobre **consultas e logs SQL**.  
Para operações completas de CRUD, veja:
- `banco-de-dados.md`
