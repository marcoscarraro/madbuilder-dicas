# 📁 Upload – Tratamento de Arquivos antes de gravar no banco  
(MadBuilder / Adianti)

Este snippet demonstra como **tratar arquivos no momento do upload**
antes de persistir os dados no banco, permitindo:

- organizar arquivos por ano/mês
- extrair nome real do arquivo
- calcular tamanho formatado
- tratar upload via JSON (padrão Adianti)

---

## 🎯 Cenário

Ao receber um upload:
- o campo vem como JSON
- o arquivo já existe temporariamente no servidor
- é necessário extrair informações antes de salvar

---

## 📂 Criar estrutura de diretórios (ano/mês)

```php
$ano = date('Y');
$mes = date('m');

$caminho_arquivo_dir = "app/output/{$ano}/{$mes}";
```

📌 Esse padrão facilita:
- organização
- manutenção
- backups
- limpeza futura

---

## 📄 Extrair informações do arquivo enviado

```php
$arquivo = DevHelper::decodeJsonUpload($data->caminho_arquivo);

$object->tamanho_arquivo = DevHelper::tamanhoArquivo($arquivo['fileName']);
$object->nome_arquivo    = DevHelper::nomeArquivo($arquivo['fileName']);
```

---

## 🧰 Funções auxiliares

Essas funções podem ficar em um **helper compartilhado**.

---

### 🔹 Retornar apenas o nome do arquivo

```php
public static function nomeArquivo($caminho)
{
    if (!$caminho) {
        return null;
    }

    return basename($caminho);
}
```

---

### 🔹 Calcular tamanho do arquivo (formatado)

```php
public static function tamanhoArquivo($caminho, $casasDecimais = 2)
{
    if (!$caminho || !file_exists($caminho)) {
        return null;
    }

    $bytes = filesize($caminho);

    if ($bytes === false) {
        return null;
    }

    $unidades = ['B', 'KB', 'MB', 'GB', 'TB'];
    $i = 0;

    while ($bytes >= 1024 && $i < count($unidades) - 1) {
        $bytes /= 1024;
        $i++;
    }

    return round($bytes, $casasDecimais) . ' ' . $unidades[$i];
}
```

---

### 🔹 Decodificar upload enviado como JSON

O Adianti envia uploads como **JSON codificado em URL**.

```php
public static function decodeJsonUpload($valor)
{
    if (!$valor || !is_string($valor)) {
        return null;
    }

    // Decodifica URL
    $json = urldecode($valor);

    // Decodifica JSON
    $dados = json_decode($json, true);

    if (json_last_error() !== JSON_ERROR_NONE) {
        return null;
    }

    return $dados;
}
```

---

## ⚠️ Observações importantes

- O arquivo precisa existir no filesystem
- Esse tratamento ocorre **antes do save**
- Ideal para preencher colunas auxiliares
- Não substitui validação de tipo/extensão

---

## 🧠 Boas práticas

- Centralize helpers de upload
- Organize arquivos por data
- Nunca confie apenas no nome do arquivo
- Valide tamanho e extensão antes de mover
- Documente estrutura de diretórios

---

## 📎 Observação final

Este arquivo documenta um **padrão robusto de tratamento de upload**
no MadBuilder / Adianti.

Veja também:
- `banco-de-dados.md`
- `infra/sessao-e-ambiente.md`
- `debug.md`
