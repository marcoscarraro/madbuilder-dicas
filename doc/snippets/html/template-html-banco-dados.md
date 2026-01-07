# 📄 Template HTML armazenado no banco + Parser  
(MadBuilder / Adianti)

Este snippet demonstra como **utilizar um template HTML salvo no banco de dados**
e realizar a **substituição automática de tags** como:

- `{$variavel}`
- `{{$tag}}`

utilizando o **AdiantiHTMLDocumentParser**, passando **diretamente um objeto**
como master, sem necessidade de criar arrays auxiliares.

Ideal para:
- contratos
- relatórios
- documentos PDF
- geração dinâmica de conteúdo

---

## 🎯 Conceito principal

O `AdiantiHTMLDocumentParser` permite:
- ler um HTML
- substituir tags automaticamente
- acessar propriedades do objeto passado via `setMaster()`

Assim, qualquer propriedade pública do objeto pode ser usada no template.

---

## 🧠 Vantagem principal

Ao usar:

```php
$html->setMaster($matricula);
```

Você pode acessar no HTML:
```html
{$nome}
{$data_hoje}
{$id}
```

sem criar arrays ou mapear variáveis manualmente.

---

## 🗃️ Fluxo da solução

1. Buscar os dados no banco
2. Injetar propriedades adicionais no objeto
3. Recuperar o HTML do banco
4. Criar um arquivo temporário
5. Processar o HTML com o parser
6. Gerar o PDF
7. Remover o arquivo temporário

---

## 🧾 Código completo

```php
$matricula = Detalhesmatricula::where('id_matricula', '=', $id_matricula)->first();

// Propriedade adicional usada no template
$matricula->data_hoje = date('d/m/Y');

// HTML vindo do banco de dados
$html_contrato = self::htmlContrato($matricula->id_curso);

// Cria arquivo temporário
$tmpFile = tempnam(sys_get_temp_dir(), '_tmp_contrato_' . $id_matricula . '_');
file_put_contents($tmpFile, $html_contrato);

// Parser do Adianti
$html = new AdiantiHTMLDocumentParser($tmpFile);
$html->setMaster($matricula);
$html->process();

// Caminho final do PDF
$document = 'app/output/contrato_' . $id_matricula . '.pdf';

// Gera o PDF
$html->saveAsPDF($document, 'A4', 'portrait');

// Remove arquivo temporário
unlink($tmpFile);

// Retorna o caminho do arquivo
if (file_exists($document)) {
    return $document;
}
```

---

## 🏷️ Exemplo de tags no HTML

```html
<p>Aluno: {$nome}</p>
<p>Data: {$data_hoje}</p>
<p>Matrícula: {$id_matricula}</p>
```

📌 As tags são resolvidas automaticamente com base nas propriedades do objeto.

---

## ⚠️ Pontos de atenção

- O objeto passado no `setMaster()` **precisa ser um `TRecord`**
- As propriedades devem ser **públicas**
- HTML deve estar **bem formado**
- Sempre remover arquivos temporários após uso

---

## 💡 Boas práticas

- Use templates armazenados no banco para flexibilidade
- Injete apenas dados necessários no objeto
- Centralize a geração do HTML em métodos reutilizáveis
- Evite lógica pesada dentro do template

---

## 📎 Observação final

Este snippet cobre:
- HTML dinâmico
- Templates no banco
- Substituição automática de tags
- Geração de PDF

Para HTML vindo de arquivos físicos, veja:
- `belement-html.md`
- `twindow-html.md`
