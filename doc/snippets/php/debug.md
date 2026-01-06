# 🐞 Debug – MadBuilder / Adianti

Este arquivo reúne ferramentas e snippets para **debug, inspeção de variáveis e análise de execução**
no **MadBuilder Framework** (baseado no Adianti).

Use estes recursos durante o desenvolvimento para entender melhor o fluxo da aplicação
e identificar problemas rapidamente.

---

## 🔍 Funções de debug nativas do MadBuilder

### `mad_dump()` – imprime e continua execução
```php
mad_dump($variavel);
```

### `md()` – atalho para `mad_dump`
```php
md($variavel);
```

### `mdd()` – imprime e interrompe execução
```php
mdd($variavel);
```

---

## 🧪 Debug de arrays e objetos

### Exibir conteúdo de `$param`
```php
mad_dump($param);
```

### Debug de objeto carregado do banco
```php
mad_dump($objeto);
```

---

## 🛑 Interromper execução manualmente

### Interromper fluxo com exceção
```php
throw new Exception('Erro durante o processamento');
```

> Útil para testar validações e fluxos condicionais.

---

## 🗃️ Debug de banco de dados

### Exibir logs SQL na tela
> Coloque logo após abrir a transação
```php
TTransaction::setLogger(new TLoggerSTD());
```

---

## 🧠 Debug de fluxo de execução

### Confirmar entrada em um método
```php
mad_dump('Entrou no método onSave');
```

### Debug condicional
```php
if ($status !== 'A') {
    mad_dump('Status inválido', $status);
}
```

---

## 🧵 Debug com JavaScript

### Executar `console.log` via TScript
```php
TScript::create("console.log('Debug JS ativo');");
```

### Inspecionar variável JS
```php
TScript::create("console.log(" . json_encode($dados) . ");");
```

---

## 🧼 Boas práticas de debug

- Remova chamadas de debug em produção
- Evite expor dados sensíveis
- Use `mdd()` apenas quando quiser interromper o fluxo
- Prefira logs em ambientes produtivos

---

## 📎 Observação final

As funções de debug do MadBuilder são ideais para desenvolvimento rápido.  
Para ambientes de produção, utilize logs estruturados.

Este arquivo cobre **apenas debug**.  
Veja também:
- `formularios.md`
- `validacoes.md`
- `mensagens-e-redirecionamento.md`
- `banco-de-dados.md`
