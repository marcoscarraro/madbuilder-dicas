# 🧾 Logs e Monitoramento SQL – MadBuilder / Adianti

Este arquivo reúne snippets para **monitoramento, depuração
e análise de SQL**, úteis principalmente durante o desenvolvimento.

⚠️ **Nunca utilize logs SQL em produção.**

---

## 🧾 Log de SQL

### Exibir SQL executado na tela

```php
TTransaction::setLogger(new TLoggerSTD());
```

📌 Esse comando exibe:
- SQL gerado
- parâmetros
- ordem de execução

---

## 🧠 Quando usar log SQL

Use apenas em:
- desenvolvimento
- depuração de filtros complexos
- validação de consultas com `TCriteria`
- análise de performance

---

## 🚫 Quando NÃO usar

- Ambiente de produção
- Processos críticos
- Execuções em massa
- Rotinas automáticas

---

## 🛑 Desativar log SQL

Basta **não definir** nenhum logger na transação:

```php
TTransaction::open(self::$database);
// consultas
TTransaction::close();
```

---

## ⚠️ Boas práticas

- Ative logs apenas temporariamente
- Nunca faça commit com logger ativo
- Use logs para entender, não para depender
- Combine com `debug.md` quando necessário

---

## 📎 Observação final

Este arquivo cobre **logs e monitoramento SQL**.  
Veja também:
- `consultas-e-filtros.md`
- `debug.md`
