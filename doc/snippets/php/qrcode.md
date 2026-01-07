# 🔳 Geração de QRCode  
(MadBuilder / Adianti)

Este snippet demonstra como **gerar um QRCode** utilizando a biblioteca
**BaconQrCode**, que já vem integrada ao **Adianti**.

⚠️ O **MadBuilder utiliza um fork do Adianti 7.5**, portanto a versão da
biblioteca **BaconQrCode é a 2.0**, e a forma de uso segue este padrão.

---

## 🎯 Cenário

Você precisa gerar um QRCode dinamicamente para:
- ingressos
- validação de acesso
- identificação de registros
- links ou códigos únicos

O conteúdo do QRCode será o valor de uma propriedade do objeto.

---

## 🧠 Conteúdo do QRCode

No exemplo abaixo, o conteúdo será:

```php
$ingresso->id
```

📌 Pode ser:
- ID
- hash
- token
- URL
- qualquer string

---

## 🧱 Código para gerar o QRCode

```php
$backend  = new \BaconQrCode\Renderer\Image\SvgImageBackEnd;

$renderer = new \BaconQrCode\Renderer\ImageRenderer(
    new \BaconQrCode\Renderer\RendererStyle\RendererStyle((int) 200, 0),
    $backend
);

$writer   = new \BaconQrCode\Writer($renderer);

// Conteúdo do QRCode
$qrcode_generated = $writer->writeString($ingresso->id);

// Converte para Base64 para uso direto em HTML
$qrcode = 'data:image/png;base64,' . base64_encode($qrcode_generated);
```

---

## 🖼️ Exibindo o QRCode no HTML

```html
<img src="{$qrcode}" alt="QR Code">
```

📌 Ideal para uso em:
- contratos
- PDFs
- telas públicas
- comprovantes

---

## ⚙️ Ajustando o tamanho do QRCode

O tamanho é definido aqui:

```php
new RendererStyle((int) 200, 0)
```

- `200` → tamanho do QRCode
- `0` → margem

Exemplo maior:
```php
new RendererStyle((int) 300, 0)
```

---

## ⚠️ Pontos importantes

- A classe usa **BaconQrCode v2.0**
- Não utilize exemplos da versão 3.x
- Pode ser usado em PDF ou HTML
- Ideal converter para Base64 para evitar arquivos físicos

---

## 💡 Boas práticas

- Use conteúdos únicos no QRCode
- Evite dados sensíveis em texto puro
- Prefira IDs, hashes ou tokens
- Centralize a geração em métodos reutilizáveis

---

## 📎 Observação final

Este snippet cobre **geração de QRCode com BaconQrCode no MadBuilder**.

Para uso em:
- contratos PDF → combine com `AdiantiHTMLDocumentParser`
- telas modais → combine com `TWindow` e `BElement`
