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

## 🎯 Validação em ações específicas

### Validar campo fora do fluxo do formulário
```php
(new TRequiredValidator)->validate('Data da aula', $dados_form->calendario_aula_id);
```

> Muito útil em ações personalizadas que não passam pelo `onSave`.

---

## 🔢 Validações de formato

### Validar valor numérico
```php
(new TNumericValidator)->validate('Quantidade', $param['quantidade']);
```

### Validar e-mail
```php
(new TEmailValidator)->validate('E-mail', $param['email']);
```

---

## 📏 Validações de tamanho

### Tamanho mínimo
```php
(new TMinLengthValidator(5))->validate('Descrição', $param['descricao']);
```

### Tamanho máximo
```php
(new TMaxLengthValidator(100))->validate('Observação', $param['observacao']);
```

---

## 🔄 Validação condicional

### Validar apenas se campo estiver preenchido
```php
if (!empty($param['cpf'])) {
    (new TRequiredValidator)->validate('CPF', $param['cpf']);
}
```

---

## 🧠 Validação personalizada

### Criar validação manual
```php
if ($param['valor'] <= 0) {
    throw new Exception('O valor deve ser maior que zero');
}
```

---

## ⚠️ Interromper execução ao validar

### Forçar erro com mensagem personalizada
```php
throw new Exception('Campos obrigatórios não foram preenchidos');
```

---

## 📎 Boas práticas

- Valide **sempre antes de salvar**
- Centralize regras de negócio quando possível
- Use mensagens claras para o usuário
- Prefira validações simples e objetivas

Este arquivo cobre **apenas validações**.  
Veja também:
- `formularios.md`
- `mensagens-e-redirecionamento.md`
- `debug.md`
- `banco-de-dados.md`
