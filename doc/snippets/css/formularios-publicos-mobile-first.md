# 📱 Formulários Públicos – CSS Mobile First  
(MadBuilder / Adianti)

Este snippet apresenta um **CSS mobile-first** voltado para
**formulários públicos**, onde:
- não há menu
- não há header de painel
- o foco é usabilidade no celular
- o formulário fica centralizado

Ideal para:
- formulários públicos
- landing pages
- validação externa
- formulários sem autenticação

---

## 🎯 Objetivo do CSS

- Remover elementos visuais desnecessários
- Centralizar o formulário
- Melhorar leitura no mobile
- Manter compatibilidade com desktop

---

## 🧱 Registro do CSS na página

```php
TPage::register_css(
    'formulario_publico_mobile',
    '
        '.card-header {
    display: none !important;
    }

    .card.panel {
    border: none !important;
    border-color: transparent !important;
    border-style: none !important;
    border-width: 0 !important;
    }

    .panel-footer.card-footer {
        display: flex;
        justify-content: space-between; /* ou center, conforme desejar */
        flex-wrap: wrap;
    }

    .form-container {
        display: flex;
        justify-content: space-between; /* ou center, conforme desejar */
        flex-wrap: wrap;
        max-width: 700px;
        margin: 0 auto;
    }
    
    .panel-footer.card-footer {
        display: flex;
        justify-content: space-between; /* ou center, conforme desejar */
        flex-wrap: wrap;
    }

    .card-footer button.btn {
        width: 90% !important;
        display: block !important;
        margin: 0 auto 7px !important;
    }
    '
);
```

---

## 📱 Comportamento Mobile First

- Mobile: ocupa 100% da largura
- Tablet/Desktop: centralizado com largura máxima
- Botões quebram linha automaticamente
- Layout flexível sem scroll horizontal

---

## 💡 Sugestões de melhoria (opcional)

### 🔹 Melhorar toque no mobile
Aumenta área clicável de inputs e botões:

```css
input, select, textarea, button {
    min-height: 44px;
}
```

---

### 🔹 Ajustar botões no mobile

```css
@media (max-width: 576px) {
    .panel-footer.card-footer button {
        width: 100%;
    }
}
```

📌 Evita botões pequenos lado a lado no celular.

---

### 🔹 Melhorar legibilidade

```css
label {
    font-size: 14px;
    font-weight: 500;
}

.form-control {
    font-size: 16px;
}
```

📌 Evita zoom automático no iOS.

---

### 🔹 Evitar campos muito próximos

```css
.form-group {
    margin-bottom: 16px;
}
```

---

## ⚠️ Observações importantes

- Ideal para páginas **sem menu**
- Não indicado para CRUD padrão interno
- Use apenas em formulários públicos
- Combine com `single-page-sem-parametros.md` para UX melhor

---

## 📎 Observação final

Este snippet cobre **CSS mobile-first para formulários públicos**.

Para formulários internos, veja:
- `formularios.md`
- `estilos-globais.md`
