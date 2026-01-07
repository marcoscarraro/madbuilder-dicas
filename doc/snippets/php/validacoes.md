# ✅ Validações – MadBuilder / Adianti

Este arquivo reúne snippets para **validação de dados em formulários e ações específicas**
utilizando os **validadores nativos do Adianti/MadBuilder**.

As validações podem ser aplicadas tanto:
- no `onSave`
- quanto em ações específicas (`onAction`, `onConfirm`, etc.)

---

## 📌 Validações obrigatórias (Required)

### Validar campo obrigatório simples
```php
(new TRequiredValidator)->validate('Forma de Pagamento', $param['tipo_pagamento_id']);
```

### Validar múltiplos campos obrigatórios
```php
(new TRequiredValidator)->validate('Parcelas', $param['parcelas']);
(new TRequiredValidator)->validate('Data dos Vencimentos', $param['data_vencimentos']);
```

---

## 📋 Validação em listas (FieldList)

### Validar campo obrigatório em `TFieldList`
```php
(new TRequiredListValidator)->validate('Quantidade', $data->qtd_ingresso_compra);
```

### Validar múltiplos campos em `TFieldList`
```php
(new TRequiredListValidator)->validate('Tipo de ingresso', $data->tipo_ingresso);
(new TRequiredListValidator)->validate('Valor', $data->valor_ingresso);
```

---

## 🧾 Validações de documentos (Brasil)

### Validar CPF
```php
(new TCPFValidator)->validate('CPF', $param['cpf']);
```

### Validar CNPJ
```php
(new TCNPJValidator)->validate('CNPJ', $param['cnpj']);
```

---

## 📧 Validações de e-mail

### Validar e-mail
```php
(new TEmailValidator)->validate('E-mail', $param['email']);
```

---

## 🔢 Validações numéricas

### Validar valor numérico
```php
(new TNumericValidator)->validate('Quantidade', $param['quantidade']);
```

### Valor mínimo permitido
```php
(new TMinValueValidator(1))->validate('Quantidade', $param['quantidade']);
```

### Valor máximo permitido
```php
(new TMaxValueValidator(100))->validate('Quantidade', $param['quantidade']);
```

---

## 📏 Validações de tamanho de texto

### Tamanho mínimo
```php
(new TMinLengthValidator(5))->validate('Descrição', $param['descricao']);
```

### Tamanho máximo
```php
(new TMaxLengthValidator(100))->validate('Observação', $param['observacao']);
```

---

## 🧹 Validações ignorando HTML (texto limpo)

### Tamanho mínimo (sem HTML)
```php
(new TStrippedTextMinLengthValidator(10))
    ->validate('Conteúdo', $param['conteudo']);
```

### Tamanho máximo (sem HTML)
```php
(new TStrippedTextMaxLengthValidator(500))
    ->validate('Conteúdo', $param['conteudo']);
```

---

## 🎯 Validação em ações específicas

### Validar campo fora do fluxo do formulário
```php
(new TRequiredValidator)->validate('Data da aula', $dados_form->calendario_aula_id);
```

---

## 🔄 Validação condicional

### Validar apenas se campo estiver preenchido
```php
if (!empty($param['cpf'])) {
    (new TCPFValidator)->validate('CPF', $param['cpf']);
}
```

```php
if (!empty($param['email'])) {
    (new TEmailValidator)->validate('E-mail', $param['email']);
}
```

---

## 🧠 Validação personalizada (manual)

```php
if ($param['valor'] <= 0) {
    throw new Exception('O valor deve ser maior que zero');
}
```

---

## ⚠️ Interromper execução ao validar

```php
throw new Exception('Campos obrigatórios não foram preenchidos');
```

---

## 📎 Boas práticas

- Valide **sempre antes de salvar**
- Prefira validadores nativos do Adianti
- Combine validações (Required + Min/Max)
- Use mensagens claras para o usuário
- Repopule o formulário em caso de erro

Este arquivo cobre **apenas validações**.  
Veja também:
- `formularios.md`
- `mensagens-e-redirecionamento.md`
- `debug.md`
- `banco-de-dados.md`
