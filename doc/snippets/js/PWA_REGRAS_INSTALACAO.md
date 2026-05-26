# Regras para um PWA ficar disponivel para instalacao

Este documento resume os requisitos que normalmente precisam ser satisfeitos para que um navegador reconheca um site como PWA instalavel e mostre a opcao **Instalar app** ou **Adicionar a tela inicial**.

> Importante: nenhum site consegue obrigar o navegador a exibir o prompt nativo de instalacao no carregamento da pagina. O navegador decide quando mostrar. Em navegadores Chromium, o evento `beforeinstallprompt` so aparece quando o navegador considera o PWA instalavel.

## Checklist principal

- O site precisa estar em HTTPS valido.
- A pagina precisa ter um manifest linkado no `<head>`.
- O manifest precisa responder com JSON valido.
- O manifest precisa conter pelo menos `name` ou `short_name`.
- O manifest precisa conter `start_url`.
- O manifest precisa conter `display` com valor como `standalone`, `fullscreen` ou `minimal-ui`.
- O manifest precisa conter icones validos, normalmente `192x192` e `512x512`.
- Os icones precisam carregar com `Content-Type: image/png` ou outro tipo correto.
- O `start_url`, `scope`, manifest e service worker precisam estar dentro da mesma origem.
- O service worker precisa registrar sem erro.
- Para melhor compatibilidade, o service worker deve ter um handler de `fetch`.
- A pagina nao pode estar em aba anonima quando voce espera comportamento completo de instalacao.
- O PWA nao pode ja estar instalado no dispositivo.
- Se o usuario recusou a instalacao recentemente, o navegador pode nao mostrar o prompt de novo por um tempo.
- No iOS/iPadOS, nao existe prompt automatico igual ao Android/Chrome; o usuario instala manualmente pelo Safari em **Compartilhar > Adicionar a Tela de Inicio**.

## Exemplo de HTML

```html
<head>
  <link rel="manifest" href="/manifest.webmanifest">
  <link rel="apple-touch-icon" href="/pwa-icon-192">
  <meta name="theme-color" content="#006bf9">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
</head>
```

## Exemplo de manifest

```json
{
  "id": "/",
  "name": "Minha Loja",
  "short_name": "Loja",
  "description": "Compre online de forma rapida.",
  "start_url": "/",
  "scope": "/",
  "display": "standalone",
  "background_color": "#006bf9",
  "theme_color": "#006bf9",
  "icons": [
    {
      "src": "/pwa-icon-192",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any"
    },
    {
      "src": "/pwa-icon-512",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any"
    }
  ],
  "screenshots": [
    {
      "src": "/pwa-screenshot-mobile",
      "sizes": "540x720",
      "type": "image/png",
      "form_factor": "narrow",
      "label": "Loja no celular"
    },
    {
      "src": "/pwa-screenshot-wide",
      "sizes": "1280x720",
      "type": "image/png",
      "form_factor": "wide",
      "label": "Tela inicial da loja"
    }
  ]
}
```

## Exemplo de service worker

```js
self.addEventListener('install', function(event) {
  self.skipWaiting();
});

self.addEventListener('activate', function(event) {
  event.waitUntil(self.clients.claim());
});

self.addEventListener('fetch', function(event) {
  event.respondWith(fetch(event.request));
});
```

## Exemplo de registro do service worker

```html
<script>
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
      navigator.serviceWorker
        .register('/serviceworker.js', { scope: '/' })
        .then(function() {
          console.log('Service Worker Registered');
        })
        .catch(function(error) {
          console.log('Service Worker registration failed', error);
        });
    });
  }
</script>
```

## Exemplo usando o prompt nativo com botao proprio

Este exemplo usa `beforeinstallprompt`. Ele e suportado principalmente em navegadores Chromium. O prompt deve ser chamado a partir de uma acao do usuario, como um clique.

```html
<button id="install-app" hidden>Instalar app</button>

<script>
  let deferredPrompt = null;
  const installButton = document.getElementById('install-app');

  window.addEventListener('beforeinstallprompt', function(event) {
    event.preventDefault();
    deferredPrompt = event;
    installButton.hidden = false;
  });

  installButton.addEventListener('click', async function() {
    if (!deferredPrompt) {
      return;
    }

    installButton.hidden = true;
    deferredPrompt.prompt();

    const choice = await deferredPrompt.userChoice;
    console.log('Resultado da instalacao:', choice.outcome);

    deferredPrompt = null;
  });

  window.addEventListener('appinstalled', function() {
    deferredPrompt = null;
    installButton.hidden = true;
    console.log('PWA instalado');
  });
</script>
```

## Exemplo confiando somente no navegador

Se voce nao quer criar botao proprio nem interceptar o prompt, nao chame `preventDefault()` no evento `beforeinstallprompt`.

```js
window.addEventListener('beforeinstallprompt', function() {
  console.log('O navegador considera este PWA instalavel.');
});
```

## Pontos que costumam impedir a instalacao

- Manifest retorna `404`, HTML ou outro conteudo que nao seja JSON.
- Manifest tem `Content-Type` incorreto.
- Icones do manifest retornam `404`.
- Icones nao sao quadrados.
- Icones nao tem os tamanhos declarados no manifest.
- `start_url` aponta para fora do `scope`.
- Service worker nao registra.
- Service worker esta em escopo diferente da pagina.
- O site usa HTTP em vez de HTTPS.
- O usuario ja instalou o app.
- O usuario recusou o prompt recentemente.
- O navegador ou sistema operacional nao suporta prompt automatico.
- No iOS, o teste foi feito em Chrome/Edge/Firefox em vez de Safari.
- A pagina esta em modo anonimo.

## Como testar

1. Abra o site em uma janela normal.
2. Limpe dados antigos do site se estiver retestando.
3. Remova o app instalado anteriormente, se existir.
4. Abra o DevTools.
5. Verifique `Application > Manifest`.
6. Confirme que o manifest nao tem erros.
7. Verifique `Application > Service Workers`.
8. Confirme que o service worker esta ativo.
9. Acesse diretamente as URLs dos icones e screenshots.
10. No Android/Chrome, confira tambem o menu de tres pontos: deve aparecer **Instalar app** ou **Adicionar a tela inicial** quando o navegador considerar o PWA instalavel.

## URLs uteis

- MDN - Making PWAs installable: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable
- MDN - Trigger install prompt: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/How_to/Trigger_install_prompt
- MDN - `beforeinstallprompt`: https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event
- web.dev - Installation: https://web.dev/learn/pwa/installation
- web.dev - Installation prompt: https://web.dev/learn/pwa/installation-prompt
- Chrome Lighthouse - Installable manifest: https://developer.chrome.com/docs/lighthouse/pwa/installable-manifest/
