# 🧩 JavaScript & UI – Ocultar, Mostrar e Desabilitar Campos  
(MadBuilder / Adianti)

Este documento reúne **snippets práticos de JavaScript integrados ao Adianti**
para **controlar visibilidade, habilitação e comportamento de campos de formulários**
em tempo de execução.

Os exemplos utilizam:
- `TScript::create()` para executar JS
- jQuery (já incluso no Adianti/MadBuilder)
- Estrutura padrão de formulários do MadBuilder

---

## 🎯 Objetivo

- Mostrar ou ocultar linhas completas do formulário
- Esconder campos com base no `name`
- Desabilitar campos
- Esconder labels
- Controlar comportamento condicional via backend (PHP → JS)

---

## 👁️ Mostrar ou ocultar linhas com base no `name` do campo

### Exemplo completo com condição

```php
if ($param['tipo_dados_ingresso'] == 'N') {

    TScript::create("$('[name=\"nome_completo_ingresso\"]').closest('.row').show()");
    TScript::create("$('[name=\"cpf_ingresso\"]').closest('.row').show()");
    TScript::create("$('[name=\"data_aniversario_ingresso\"]').closest('.row').show()");
    TScript::create("$('[name=\"whatsapp_ingresso\"]').closest('.row').show()");
    TScript::create("$('[name=\"email_ingresso\"]').closest('.row').show()");
    TScript::create("$('[name=\"cidade_id_ingresso\"]').closest('.row').show()");

} else {

    TScript::create("$('[name=\"nome_completo_ingresso\"]').closest('.row').hide()");
    TScript::create("$('[name=\"cpf_ingresso\"]').closest('.row').hide()");
    TScript::create("$('[name=\"data_aniversario_ingresso\"]').closest('.row').hide()");
    TScript::create("$('[name=\"whatsapp_ingresso\"]').closest('.row').hide()");
    TScript::create("$('[name=\"email_ingresso\"]').closest('.row').hide()");
    TScript::create("$('[name=\"cidade_id_ingresso\"]').closest('.row').hide()");
}
```

📌 **O que este código faz**  
- Localiza o campo pelo atributo `name`
- Sobe até a `.row` mais próxima
- Oculta ou exibe a linha inteira do formulário

---

## 🧱 Ocultar apenas o container do campo

Quando o formulário usa layout inline (`fb-inline-field-container`):

```php
TScript::create(
    "$('[name=\"nome_aluno\"]').closest('.fb-inline-field-container').hide()"
);
```

📌 Ideal para:
- Layouts responsivos
- Formulários públicos
- Campos inline do MadBuilder

---

## 🚫 Desabilitar um campo (readonly visual + backend)

```php
TEntry::disableField(self::$formName, 'nome_aluno');
```

📌 Observações:
- O campo fica desabilitado no frontend
- O valor **não é enviado no submit**
- Ideal para campos informativos

---

## 🏷️ Ocultar um label específico

### Ocultar label pelo texto exibido

```php
TScript::create(
    "$('label:contains(\"Nome do aluno\")').hide();"
);
```

📌 Atenção:
- O texto deve ser **exatamente igual**
- Sensível a idioma e acentuação
- Útil quando não se tem controle direto do HTML

---

## 🔄 Combinação comum: ocultar campo + label

```php
TScript::create("$('[name=\"nome_aluno\"]').closest('.row').hide()");
TScript::create("$('label:contains(\"Nome do aluno\")').hide()");
```

---

## 💡 Boas práticas

- Prefira ocultar o **container (`.row`)**, não apenas o input
- Use `disableField` quando o dado não deve ser enviado
- Centralize regras de exibição no backend sempre que possível
- Evite depender apenas do texto do label em sistemas multilíngues
- Teste sempre em desktop e mobile

---

## ⚠️ Observações importantes

- `TScript::create()` injeta JS diretamente na página
- O código JS é executado **após o carregamento do formulário**
- Certifique-se que o campo já existe antes de executar o script

---

## 📎 Relacionado

Veja também:
- `formularios.md`
- `validacoes.md`
- `mensagens-e-redirecionamento.md`
- `css-e-layout.md`
