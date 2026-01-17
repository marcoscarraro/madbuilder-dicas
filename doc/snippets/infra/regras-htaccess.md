# 🌐 Snippets – Regras .htaccess (Adianti / MadBuilder)

Este documento reúne **padrões de regras Rewrite** utilizadas em projetos
com **Adianti Framework / MadBuilder (v7.5)**.

O objetivo é:
- criar URLs amigáveis
- mapear URLs para classes e métodos
- suportar chamadas públicas e XHR
- manter padrão e organização

---

## ⚙️ Configuração básica obrigatória

```apache
RewriteEngine On
```

📌 Deve estar no início do `.htaccess`.

---

## 🛒 Rotas públicas simples (sem parâmetros)

### Exemplo: Compra de ingressos

```apache
RewriteRule ^compra$ index.php?class=VendaIngressoPublicEtapa1form&method=onShow&%{QUERY_STRING} [NC]
RewriteRule ^xhr-compra$ engine.php?class=VendaIngressoPublicEtapa1form&method=onShow&%{QUERY_STRING} [NC]
```

📌 Padrão recomendado:
- URL limpa para acesso público
- versão `xhr-` para chamadas AJAX (`engine.php`)

---

### Exemplo: Devolução

```apache
RewriteRule ^devolucao$ index.php?class=DevolucaoIngressosForm&method=onShow&%{QUERY_STRING} [NC]
RewriteRule ^xhr-devolucao$ engine.php?class=DevolucaoIngressosForm&method=onShow&%{QUERY_STRING} [NC]
```

---

## 🔓 Rotas públicas de autenticação

```apache
# Nova conta
RewriteRule ^nova-conta$ index.php?class=NovaContaForm&%{QUERY_STRING} [NC]
RewriteRule ^xhr-nova-conta$ engine.php?class=NovaContaForm&%{QUERY_STRING} [NC]

# Esqueci senha
RewriteRule ^esqueci-senha$ index.php?class=EsqueciSenhaForm&%{QUERY_STRING} [NC]
RewriteRule ^xhr-esqueci-senha$ engine.php?class=EsqueciSenhaForm&%{QUERY_STRING} [NC]

# Login
RewriteRule ^login$ index.php?class=LoginForm&%{QUERY_STRING} [NC]
RewriteRule ^xhr-login$ engine.php?class=LoginForm&%{QUERY_STRING} [NC]
```

📌 Ideal para:
- páginas públicas
- formulários sem autenticação
- rotas amigáveis para SEO

---

## 🔢 Rotas com parâmetros na URL

### Parâmetro numérico ou composto

```apache
RewriteRule ^cad_membros-([0-9_-]+)$ index.php?class=CadastroPessoaPublico&method=onShow&unidade=$1 [NC]
```

📌 Exemplo de URL:
```
/cad_membros-10
/cad_membros-1_2
```

📌 O valor capturado fica disponível em:
```php
$param['unidade'];
```

---

### Parâmetro alfanumérico

```apache
RewriteRule ^projeto-([A-Za-z0-9]*)$ index.php?class=ProdutosList&method=onShow&unid=$1&%{QUERY_STRING} [NC]
```

📌 Exemplo:
```
/projeto-ABC123
```

---

## 🧩 Rotas com múltiplos parâmetros

```apache
RewriteRule ^evento-([0-9]+)-([A-Za-z0-9_-]+)$ index.php?class=EventoPublicView&method=onShow&id=$1&slug=$2 [NC]
```

📌 Exemplo:
```
/evento-15-show-rock
```

📌 Parâmetros disponíveis:
```php
$param['id'];
$param['slug'];
```

---

## ⚡ Rotas XHR (AJAX) – padrão recomendado

Sempre que a rota for usada via JavaScript ou ações assíncronas:

```apache
RewriteRule ^xhr-nome-da-rota$ engine.php?class=MinhaClasse&method=minhaAcao&%{QUERY_STRING} [NC]
```

📌 Benefícios:
- melhor separação de responsabilidades
- evita problemas de renderização
- segue padrão interno do Adianti

---

## 🛡️ Boas práticas

- Sempre use `[NC]` (case insensitive)
- Prefira nomes de URL em **kebab-case**
- Separe rotas públicas das autenticadas
- Use `engine.php` apenas para XHR
- Evite regras genéricas demais

---

## 🚫 O que evitar

❌ URLs com nomes de classe explícitos  
❌ Expor métodos sensíveis via URL pública  
❌ Regras ambíguas que capturam tudo  

---

## 📎 Observações finais

- As regras funcionam tanto em Apache quanto em ambientes compatíveis
- Em produção, valide conflitos com outras regras existentes
- Documente novas rotas adicionadas ao projeto

Este arquivo faz parte da pasta **infra/**.