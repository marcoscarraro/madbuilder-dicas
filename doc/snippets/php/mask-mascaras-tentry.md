# 🎭 Snippets – Máscaras de Campos (TEntry)

Este arquivo reúne **exemplos práticos de máscaras**
utilizando `TEntry::changeMask` no **MadBuilder / Adianti**.

As máscaras ajudam a:
- padronizar entrada de dados
- reduzir erros do usuário
- melhorar a experiência (UX)

---

## 📞 Telefone

### Telefone com DDD (fixo)
```php
TEntry::changeMask(self::$formName, 'telefone', '(99) 9999-9999');
```

### Celular com DDD
```php
TEntry::changeMask(self::$formName, 'celular', '(99) 99999-9999');
```

---

## 🗓️ Datas

### Data (DD/MM/AAAA)
```php
TEntry::changeMask(self::$formName, 'data_nascimento', '99/99/9999');
```

### Mês/Ano
```php
TEntry::changeMask(self::$formName, 'competencia', '99/9999');
```

---

## 🪪 Documentos

### CPF
```php
TEntry::changeMask(self::$formName, 'cpf', '999.999.999-99');
```

### CNPJ
```php
TEntry::changeMask(self::$formName, 'cnpj', '99.999.999/9999-99');
```

### CPF ou CNPJ (definir dinamicamente via JS)
```php
        // Adiciona o comportamento dinâmico via jQuery
        TScript::create("
            $('[name=\"cpf_cnpj\"]').on('keyup', function() {
                var digitos = $(this).val().replace(/\D/g, '');
                var total   = digitos.length;

                if (total > 11 && total <= 14) {
                    $(this).mask('00.000.000/0000-00');    
                } else if (total => 1 && total <= 11) {
                    $(this).mask('000.000.000-00');
                } else {
                    $(this).unmask();
                }
            });
        ");
```

📌 Para CPF/CNPJ dinâmico, recomenda-se usar JS para alternar a máscara.

---

## 🏠 Endereço

### CEP
```php
TEntry::changeMask(self::$formName, 'cep', '99999-999');
```

---

## 🚗 Veículos

### Placa de carro (padrão antigo)
```php
TEntry::changeMask(self::$formName, 'placa', 'AAA-9999');
```

### Placa Mercosul
```php
TEntry::changeMask(self::$formName, 'placa', 'AAA9A99');
```

---

## 🔢 Valores numéricos

### Número inteiro
```php
TEntry::changeMask(self::$formName, 'quantidade', '999999');
```

### Código ou chave alfanumérica
```php
TEntry::changeMask(self::$formName, 'chave_acesso', 'AAAA-AAAA');
```

### Código longo
```php
TEntry::changeMask(self::$formName, 'codigo', 'AAAA-AAAA-AAAA');
```

---

## 🧠 Observações importantes

- `9` → aceita apenas números  
- `A` → aceita apenas letras  
- `*` → aceita letras e números  

📌 A máscara **não valida o conteúdo**, apenas o formato.  
Use **validadores** para garantir a integridade dos dados.

---

## ✅ Boas práticas

- Sempre combine máscara + validação
- Não use máscara em campos que recebem dados já tratados
- Evite máscaras em campos de busca
- Prefira clareza ao invés de excesso de formatação

---

## 📎 Veja também

- `validacoes.md`
- `formularios.md`
- `javascript.md`
- `banco-de-dados.md`
