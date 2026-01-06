# 🛠️ Sessão e Ambiente – Infra (MadBuilder / Adianti)

Este arquivo reúne snippets e boas práticas para **manipulação de sessão**,
**variáveis de ambiente**, **contexto da aplicação** e controle de estado
no **MadBuilder Framework** (baseado no Adianti).

Esses recursos são fundamentais para controle de usuário, permissões
e comportamento da aplicação.

---

## 🧠 Sessão (TSession)

### Definir valor na sessão
```php
TSession::setValue('usuario_logado', $usuario);
```

### Recuperar valor da sessão
```php
$usuario = TSession::getValue('usuario_logado');
```

### Remover valor específico da sessão
```php
TSession::delValue('usuario_logado');
```

### Limpar toda a sessão
```php
TSession::freeSession();
```

---

## 🔐 Controle de autenticação

### Verificar se usuário está logado
```php
if (!TSession::getValue('usuario_logado')) {
    throw new Exception('Usuário não autenticado');
}
```

---

## 🌍 Ambiente da aplicação

### Identificar classe atual
```php
$class = _CLASS_;
```

### Identificar método atual
```php
$method = $_REQUEST['method'] ?? null;
```

---

## ⚙️ Variáveis globais comuns

### Acessar parâmetros da requisição
```php
$param['id'];
```

### Acessar variáveis globais PHP
```php
$_SERVER['HTTP_HOST'];
$_SERVER['REQUEST_URI'];
```

---

## 🧪 Controle de ambiente (dev / prod)

### Exemplo de uso condicional
```php
if (TSession::getValue('environment') === 'development') {
    TTransaction::setLogger(new TLoggerSTD());
}
```

---

## 🛑 Encerrar sessão do usuário

### Logout manual
```php
TSession::freeSession();
AdiantiCoreApplication::gotoLoginPage();
```

---

## 🧠 Boas práticas

- Evite armazenar objetos muito grandes na sessão
- Nunca salve dados sensíveis sem necessidade
- Limpe sessão no logout
- Use sessão apenas para estado temporário

---

## 📎 Observação final

Este arquivo cobre **sessão e ambiente da aplicação**.  
Veja também:
- `mensagens-e-redirecionamento.md`
- `banco-de-dados.md`
