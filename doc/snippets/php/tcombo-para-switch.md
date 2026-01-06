# 🧩 Componentes Especiais – MadBuilder / Adianti

Este arquivo reúne snippets avançados para **customização de componentes**
no **MadBuilder / Adianti**, com foco em **melhoria de UX**
e adaptação visual de campos padrão.

O exemplo principal aborda a **conversão de um `TCombo` em um switch visual**
dentro de um **TFieldList**, mantendo compatibilidade total com o backend.

---

## 🎯 Cenário de uso

- Um `TCombo` dentro de um `TFieldList`
- Necessidade de melhorar a experiência do usuário
- Representar opções como **switch visual**
- Manter o `select` funcional para envio e validação

---

## 🎛️ Converter `TCombo` em Switch (TFieldList)

A conversão é feita **somente no frontend**:
- o `select` continua existindo
- o switch é apenas uma representação visual

---

## 🎨 CSS do Switch

> Deve ser registrado **após a criação da página**

```php
//<onAfterPageCreation>
TPage::register_css(
    'switch-select',
    '
    select.switch-hidden {
        position: absolute;
        opacity: 0;
        pointer-events: none;
    }

    .switch-container {
        display: inline-flex;
        align-items: center;
        background: #e0e0e0;
        border-radius: 3px;
        padding: 3px;
        cursor: pointer;
        transition: background-color 0.3s ease;
        user-select: none;
    }

    .switch-option {
        padding: 8px 12px;
        border-radius: 2px;
        font-size: 14px;
        font-weight: 500;
        transition: all 0.3s ease;
        color: #666;
        white-space: nowrap;
    }

    .switch-option.active {
        background: #4CAF50;
        color: white;
        box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }

    .switch-container.falta-active {
        background: #ffebee;
    }

    .switch-option.active.falta {
        background: #f44336;
    }

    .switch-option:hover:not(.active) {
        color: #333;
    }
    '
);
```

---

## ⚙️ JavaScript – Conversão do Select para Switch

```php
TScript::create("
function convertSelectToSwitch(selector) {
    document.querySelectorAll(selector).forEach(function(select) {
        if (select.classList.contains('switch-converted')) return;

        select.classList.add('switch-converted', 'switch-hidden');

        const container = document.createElement('div');
        container.className = 'switch-container';

        Array.from(select.options).forEach(function(option) {
            const switchOption = document.createElement('div');
            switchOption.className = 'switch-option';
            switchOption.textContent = option.text;
            switchOption.dataset.value = option.value;

            if (option.value === 'F') {
                switchOption.classList.add('falta');
            }

            if (option.selected) {
                switchOption.classList.add('active');
                if (option.value === 'F') {
                    container.classList.add('falta-active');
                }
            }

            switchOption.addEventListener('click', function() {
                container.querySelectorAll('.switch-option').forEach(function(opt) {
                    opt.classList.remove('active');
                });

                switchOption.classList.add('active');
                select.value = switchOption.dataset.value;
                select.dispatchEvent(new Event('change', { bubbles: true }));

                if (switchOption.dataset.value === 'F') {
                    container.classList.add('falta-active');
                } else {
                    container.classList.remove('falta-active');
                }
            });

            container.appendChild(switchOption);
        });

        select.parentNode.insertBefore(container, select.nextSibling);
    });
}

/* Função global para reconversão */
window.reconvertSwitches = function() {
    convertSelectToSwitch('select.switch-select:not(.switch-converted)');
};

/* Converte os switches iniciais */
window.reconvertSwitches();
");
```

---

## 🔄 Reconverter após atualização dinâmica (TFieldList)

Sempre que o `TFieldList` for atualizado (ex: `sendData`),
os switches precisam ser **recriados manualmente**.

### Usar após `sendData`
```php
TScript::create("
setTimeout(function() {

    /* Remove switches antigos */
    document.querySelectorAll('.switch-container').forEach(function(el) {
        el.remove();
    });

    /* Remove marcação de convertido */
    document.querySelectorAll('select.switch-converted').forEach(function(select) {
        select.classList.remove('switch-converted', 'switch-hidden');
    });

    /* Reconverte todos os selects */
    window.reconvertSwitches();

}, 700);
");
```

---

## ⚠️ Observações importantes

- O `select` **continua sendo o campo real**
- O switch é apenas visual
- Essencial para `TFieldList` dinâmico
- Sempre reconverter após `sendData`
- A classe `switch-select` é obrigatória no `TCombo`

---

## 🧠 Boas práticas

- Use esse padrão apenas quando melhorar a UX
- Documente o comportamento diferenciado
- Teste com múltiplas linhas no `TFieldList`
- Evite lógica de negócio no JavaScript

---

## 📎 Observação final

Este arquivo documenta um **padrão avançado de frontend**
para o MadBuilder / Adianti.

Veja também:
- `formularios.md`
- `scripts-jquery.md`
- `carregamento-dinamico.md`
