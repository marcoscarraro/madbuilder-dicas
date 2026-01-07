# 📱 iOS + HTTP/2 + Nginx Proxy Manager  
(Solução de compatibilidade)

Este arquivo documenta um **problema conhecido de compatibilidade**
entre **iOS (Safari / WebView)** e servidores web configurados com **HTTP/2**
através do **Nginx Proxy Manager**.

Em alguns cenários, o site:
- não carrega corretamente no iOS
- apresenta falhas intermitentes
- funciona normalmente em Android e desktop

---

## 🚨 Problema

Dispositivos **iOS** podem apresentar instabilidade
quando o servidor está atrás de um proxy
com **HTTP/2 habilitado**, especialmente com SSL gerenciado pelo proxy.

---

## ✅ Solução aplicada

A solução envolve **três etapas obrigatórias**:

---

## 1️⃣ Desabilitar HTTP/2 no Nginx Proxy Manager

No proxy configurado:
- desative o suporte a **HTTP/2**
- salve as configurações

Isso força o uso de **HTTP/1.1**, que é mais estável para iOS nesses casos.

---

## 2️⃣ Gerar novamente os certificados SSL

Após desabilitar o HTTP/2:
- gere novamente os certificados SSL
- reaplique o certificado ao proxy

⚠️ Isso garante que o SSL fique alinhado com o novo protocolo.

---

## 3️⃣ Configuração avançada (Advanced)

Na aba **Advanced** do proxy, adicione as seguintes diretivas:

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_hide_header Upgrade;
```

---

## 🧠 O que essas diretivas resolvem

- Mantêm o host original
- Preservam o IP real do cliente
- Evitam conflitos de upgrade de protocolo
- Melhoram compatibilidade com WebView iOS

---

## ⚠️ Observações importantes

- Esse ajuste é específico para ambientes com proxy reverso
- Não afeta negativamente Android ou desktop
- Ideal para aplicações web, sistemas e PWA
- Sempre teste após mudanças de proxy

---

## 🧠 Boas práticas

- Documente alterações de infra
- Evite HTTP/2 se não houver ganho real
- Teste sempre em iOS real (Safari)
- Combine com logs do Nginx para diagnóstico

---

## 📎 Observação final

Este snippet documenta uma **solução prática de infraestrutura**
para problemas com iOS e HTTP/2.

Veja também:
- `infra/sessao-e-ambiente.md`
- `infra/ambiente-producao.md` (se existir)
