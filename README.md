# Casa 3D Interativa — Widget Embedável

Uma casa 3D sólida feita em [Three.js](https://threejs.org/) que pode ser embutida em
qualquer site ou LMS (Moodle, Canvas, etc.).

## O que ela faz

- **Arrastar com o mouse** → gira a casa (com inércia suave).
- **Clicar na porta** → abre/fecha com animação.
- **Clicar na janela** → abre/fecha com animação.
- **Hover** sobre porta/janela → destaca e mostra o cursor de "clicável".
- Funciona no **mobile** (toque pra girar e clicar).
- É **responsiva** (preenche o espaço do container).

Tudo está num único arquivo: **`index.html`**. O Three.js é carregado de um CDN, então
**não precisa de build, npm nem instalação**.

---

## Como usar

### 1. Hospede o `index.html`

Coloque o `index.html` em qualquer lugar que sirva arquivos por HTTPS, por exemplo:

- GitHub Pages, Netlify, Vercel, Cloudflare Pages (todos gratuitos)
- A própria área de arquivos / mídia do LMS, se ela der uma URL pública
- Qualquer hospedagem de site que você já tenha

Anote a URL final, ex.: `https://seu-dominio.com/casa3d/index.html`

### 2. Embute com `<iframe>` (método recomendado)

Este é o jeito **mais compatível** — a maioria dos LMS aceita iframe e bloqueia `<script>`
solto. Cole isto no editor de conteúdo (no modo HTML):

```html
<iframe
  src="https://seu-dominio.com/casa3d/index.html"
  width="100%" height="480"
  style="border:0; max-width:720px; border-radius:12px;"
  loading="lazy"
  title="Casa 3D interativa"></iframe>
```

Ajuste `height` e `max-width` como preferir.

---

## Alternativa: embed inline (sem iframe)

Se o seu LMS/site **permite colar HTML com `<script>`** (alguns permitem, muitos sanitizam),
você pode embutir direto sem hospedar um arquivo separado. Copie o conteúdo das tags
`<style>`, do `<div id="house3d-root">`, do `<script type="importmap">` e do
`<script type="module">` do `index.html` para dentro da sua página.

> ⚠️ **Atenção:** Moodle, Canvas e a maioria dos LMS **removem `<script>`** do conteúdo por
> segurança. Se a casa não aparecer ao colar inline, use o método do **iframe** acima — é o
> caminho que sempre funciona.

---

## Personalização rápida

No `index.html`, dentro do `<script type="module">`, dá pra mexer fácil em:

| O que mudar            | Onde                                                            |
|------------------------|----------------------------------------------------------------|
| Tamanho da casa        | constantes `W`, `H`, `D`                                       |
| Cor das paredes        | `bodyMat` (`color`)                                            |
| Cor do telhado         | `roofMat` (`color`)                                            |
| Cor de fundo           | `background` do `#house3d-root` no CSS                         |
| Quanto a porta abre    | constante `OPEN_DOOR`                                          |
| Quanto a janela abre   | constante `OPEN_WIN`                                           |
| Velocidade da animação | variável `k` no loop `animate()` (maior = mais rápido)        |
| Zoom mín/máx           | `controls.minDistance` / `controls.maxDistance`               |

---

## Observações técnicas

- **Versão do Three.js fixada** (`three@0.160.0`) no `importmap` pra não quebrar com
  atualizações do CDN.
- **CSP restritivo:** se o site/LMS tiver Content-Security-Policy que bloqueie CDNs externos,
  baixe `three.module.js` e `OrbitControls.js` e ajuste o `importmap` pra apontar pros arquivos
  locais.
- **WebGL:** se o navegador não suportar WebGL, aparece uma mensagem de fallback no lugar.
- **Acessibilidade:** respeita `prefers-reduced-motion` — com essa preferência ativa, porta e
  janela abrem sem animação e a rotação não tem inércia.
