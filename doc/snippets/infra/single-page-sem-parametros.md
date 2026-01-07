# 🧭 Aplicação Single Page (Ocultar parâmetros na URL)  
(MadBuilder / Adianti)

Este snippet demonstra como **desabilitar o controle de estado da URL**
no Adianti/MadBuilder, fazendo com que a aplicação funcione
como uma **Single Page Application (SPA)**.

Com isso:
- parâmetros não aparecem na URL
- navegação fica mais limpa
- evita exposição de `class`, `method` e `params`

---

## 🎯 Cenário

Por padrão, o Adianti:
- registra estado da navegação
- inclui parâmetros na URL
- permite navegação via histórico do navegador

Em alguns projetos, isso **não é desejável**.

---

## ⚙️ Configuração necessária

Edite o arquivo:

```text
app/lib/include/application.js
```

Adicione (ou altere) a seguinte configuração:

```javascript
Adianti.registerState = false;
```

---

## 🧠 O que essa configuração faz

- Desativa o registro de estado da navegação
- Oculta parâmetros passados via URL
- Evita URLs extensas e expostas
- Aproxima o comportamento de uma SPA

---

## ⚠️ Observações importantes

- O histórico do navegador fica limitado
- Não é possível compartilhar URLs com estado interno
- Ideal para sistemas internos e administrativos
- Não recomendado para aplicações que dependem de deep link

---

## 🧠 Boas práticas

- Avalie necessidade de histórico antes de desativar
- Documente essa decisão no projeto
- Combine com controle de sessão adequado
- Teste navegação em múltiplos cenários

---

## 📎 Observação final

Este arquivo documenta um ajuste de **infraestrutura e navegação**
no MadBuilder / Adianti.

Veja também:
- `infra/sessao-e-ambiente.md`
- `infra/ios-http2-nginx.md`
- `scripts-jquery.md`
