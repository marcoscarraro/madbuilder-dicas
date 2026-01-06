# 🧩 BElement carregando HTML – MadBuilder / Adianti

Este arquivo reúne snippets e boas práticas para **uso do BElement**
em conjunto com **HTML externo**, **THtmlRenderer** e **conteúdo dinâmico**
no **MadBuilder / Adianti**.

Esse padrão é muito usado para:
- banners
- blocos informativos
- conteúdo institucional
- layouts híbridos (HTML + PHP)

---

## 🧱 Criar um BElement do tipo `div`

Adicione um `BElement` na sua página para servir como container do HTML.

```php
$banner_comprar_ingresso = new BElement('div');
$banner_comprar_ingresso->id = 'banner_comprar_ingresso';
```

Esse elemento será usado como **placeholder** para o conteúdo HTML.

---

## 📄 Criar o arquivo HTML externo

Crie um arquivo HTML com o conteúdo desejado, por exemplo:

**`app/resources/bannerComprarIngresso.html`**

```html
<div class="banner">
    <h2>Compre seu ingresso</h2>
    <p>Garanta sua participação agora mesmo.</p>
</div>
```

---

## 🧩 Inserir HTML usando `THtmlRenderer`

A inserção do HTML **deve ser feita dentro do bloco especial**:

```php
//<onBeforeAddFieldsToForm>
```

Esse ponto garante que o HTML seja renderizado
**antes do formulário ser exibido**.

### Exemplo completo
```php
//<onBeforeAddFieldsToForm>

$bannerHtmlFile = new THtmlRenderer('app/resources/bannerComprarIngresso.html');
$bannerHtmlFile->enableSection('main');

$banner_comprar_ingresso->add($bannerHtmlFile);
```

---

## 🔄 Atualizar o conteúdo do BElement dinamicamente

Se for necessário alterar o conteúdo do `BElement` em tempo de execução,
é **obrigatório limpar o conteúdo anterior**, senão o sistema irá **incrementar**.

### Limpar e adicionar novo conteúdo
```php
$this->banner_comprar_ingresso->children = [];
$this->banner_comprar_ingresso->add('testeeeeeeeeeeeeeeeeeeee');
```

---

## ⚠️ Atenção: métodos estáticos

O acesso direto via `$this` **só funciona em métodos não estáticos**.

Se o método que atualiza o conteúdo for **estático**, faça assim:

```php
$self = new self($param);
$self->banner_comprar_ingresso->children = [];
$self->banner_comprar_ingresso->add('Novo conteúdo');
```

---

## 🎨 Aplicar estilos ao BElement

### Classe CSS
```php
$banner_comprar_ingresso->class = 'banner-container';
```

### Estilo inline
```php
$banner_comprar_ingresso->style = 'margin: 10px 0;';
```

---

## ⚠️ Observações importantes

- Sempre use `<onBeforeAddFieldsToForm>` para HTML externo
- Limpe `children` antes de adicionar novo conteúdo
- Evite lógica de negócio dentro do HTML
- Prefira HTML externo para layouts grandes
- Atenção ao uso em métodos estáticos

---

## 🧠 Boas práticas

- Use `BElement` apenas como container visual
- Centralize estilos no CSS global
- Use `THtmlRenderer` para reutilização
- Documente o comportamento dinâmico do componente

---

## 📎 Observação final

Este arquivo cobre o uso completo de **BElement + HTML externo**.  
Veja também:
- `estilos-globais.md`
- `scripts-jquery.md`
- `carregamento-dinamico.md`
