# 🗃️ Banco de Dados – MadBuilder / Adianti

Este arquivo reúne snippets para **operações com banco de dados**, uso de **Active Record**,
**transações**, **consultas**, **logs SQL** e boas práticas no
**MadBuilder Framework** (baseado no Adianti).

---

## 🔌 Transações e conexão

### Abrir e fechar conexão
```php
TTransaction::open(self::$database); // Inicia conexão
TTransaction::close();               // Fecha conexão
```

### Conectar em outro banco
```php
TTransaction::open(MAIN_DATABASE);
```

---

## 📝 Logs de SQL

### Exibir SQL executado na tela
> Colocar logo após abrir a transação
```php
TTransaction::setLogger(new TLoggerSTD());
```

---

## 💾 Gravar registros (Create)

### Criar novo registro
```php
TTransaction::open(self::$database);

$objeto = new TurmaConteudos();
$objeto->turma = 'Turma XPTO';
$objeto->store();

TTransaction::close();
```

---

## ✏️ Atualizar registros (Update)

### Atualizar registro informando ID
```php
TTransaction::open(self::$database);

$objeto = new TurmaConteudos();
$objeto->id = 1;
$objeto->turma = 'Turma XPTO Atualizada';
$objeto->store();

TTransaction::close();
```

---

## 🔍 Buscar registros

### Buscar por chave primária (ID)
```php
Curso::find($cursoId);
```

### Buscar por outra coluna
```php
Calendario::where(
    'data_calendario',
    '=',
    $dataAula->format('Y-m-d')
)->load();
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
->load()     // Carrega todos os registros
->first()    // Retorna o primeiro registro
->last()     // Retorna o último registro
->orderBy('ordem', 'asc') // Ordena resultados
```

---

## 🔢 Contagem de registros

```php
$matriculados = Matricula::where(
    'turma_id',
    '=',
    $data->turma_id
)->count();
```

---

## 🔗 Relacionamento entre objetos

### Acessar dados relacionados
Se a tabela possui a FK corretamente definida:
```php
echo $telefone->pessoa->nome_completo;
```

⚠️ O relacionamento só funciona se a **chave estrangeira existir na tabela atual**.

---

## 🛑 Tratamento de erros com transação

```php
try {
    TTransaction::open(self::$database);

    $objeto = new Registro();
    $objeto->nome = 'Teste';
    $objeto->store();

    TTransaction::close();
} catch (Exception $e) {
    TTransaction::rollback();
    throw $e;
}
```

---

## 🧠 Boas práticas

- Sempre use `try/catch` com `rollback`
- Abra a transação o mais tarde possível
- Feche a transação o quanto antes
- Evite lógica complexa dentro da transação
- Use logs SQL apenas em desenvolvimento

---

## 📎 Observação final

Este arquivo cobre **operações de banco de dados** com Active Record.  
Para consultas mais avançadas, utilize `TCriteria`, `TFilter` e `TRepository`.

Veja também:
- `formularios.md`
- `validacoes.md`
- `debug.md`
- `mensagens-e-redirecionamento.md`
