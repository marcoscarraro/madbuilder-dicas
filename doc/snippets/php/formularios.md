# 🧾 Formulários – MadBuilder / Adianti

Este arquivo reúne **snippets práticos para criação, manipulação e envio de dados em formulários**
utilizando o **MadBuilder Framework** (baseado no Adianti).

Os exemplos abaixo são pensados para uso real no dia a dia.

---

## 📌 Aplicar máscara em campos

### Aplicar máscara em um campo específico
```php
TEntry::changeMask(self::$formName, 'chave_acesso', 'AAAA-AAAA');
```

---

## 🎨 Estilos em campos e elementos do formulário

### Adicionar estilo inline em um elemento
> Deve ser feito **antes de montar a página**
```php
$text31->style = 'float: right !important;';
```

### Alterar estilo (CSS) de um campo específico
```php
TPage::register_css(
    'upperCaseChaveAcesso',
    'input[name="chave_acesso"] { text-transform: uppercase; }'
);
```

### Alterar estilo CSS global da página
```php
$class = _CLASS_;
$css = "
    div[page-name='{$class}'] .card-header.panel-heading {
        position: sticky;
        top: 0;
        z-index: 1000;
    }
";
parent::register_css("my_" . _CLASS_, $css);
```

### Informar uma classe CSS para uma `$row` específica
```php
$row3->class = 'row-full-width';

TPage::register_css(
    'row-full-width',
    '.row-full-width .fb-inline-field-container {
        width: 100% !important;
        float: none !important;
        display: block !important;
    }'
);
```

---

## 📤 Enviar dados para formulários

### Enviar dados para um formulário existente
```php
$objetoFormulario = new stdClass();
$objetoFormulario->data_termino = $dataAula->format('d/m/Y');

TForm::sendData(self::$formName, $objetoFormulario);
```

---

## 📥 Receber dados do formulário

### Valores do formulário chegam via `$param`
```php
$param['curso_id'];
```

> Os dados vêm como **array associativo**, conforme o `name` dos campos.

---

## 🔄 Submeter formulário via código

### Fazer post de formulário via `TApplication`
```php
// TApplication::postData($formName, $class, $method = NULL, $parameters = NULL);
TApplication::postData('form_interaction', 'FormInteractionsView', 'onView');
```

---

## 🔗 Relacionamento de objetos (Form + Model)

### Acessar relacionamento entre objetos
Se a tabela **Telefone** possui uma FK `pessoa_id`:
```php
echo $telefone->pessoa->nome_completo;
```

⚠️ **Importante:**  
O relacionamento só funciona se a **chave estrangeira existir na tabela atual**.

Exemplo que **não funciona**:
```php
// Isso NÃO funciona se a FK estiver apenas em Telefone
echo $pessoa->telefone->numero;
```

---

## 🔢 Formatação de valores

### Formatar valor monetário brasileiro
```php
number_format($valor, 2, ',', '.');
```

### Converter valor monetário para salvar no banco (double)
```php
(double) str_replace(',', '.', str_replace('.', '', $param['valor_pago']));
```

---

## 🪟 Ações de interface relacionadas a formulários

### Fechar janela (modal / window)
```php
TWindow::closeWindow();
```

### Fechar cortina lateral
```php
TScript::create("Template.closeRightPanel()");
```

---

## ⚠️ Interromper execução com erro
```php
throw new Exception('O instrutor não está disponível neste período');
```

---

## Para retornar os dados para o formulário após catch (Exception $e) basta usar o código abaixo dentro do catch
```php
$this->form->setData( $this->form->getData() );
```
---

## 📎 Observações finais

- Sempre valide os dados antes de salvar
- Prefira enviar dados com `TForm::sendData`
- Centralize regras de negócio fora do formulário quando possível

Este arquivo cobre **apenas formulários**.  
Veja também:
- `validacoes.md`
- `mensagens-e-redirecionamento.md`
- `banco-de-dados.md`
