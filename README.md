# 🥟 Tequeño Loco — PWA de vendas

Aplicação Web Progressiva (PWA) **mobile-first** para uma hamburgueria fictícia de tequeños:
cardápio com filtros e busca, carrinho persistente, cupons de desconto, checkout com
finalização pelo WhatsApp, histórico de pedidos, tema claro/escuro e **instalação na tela
inicial** com funcionamento offline.

> Projeto de estudo de front-end. Nenhum dado é enviado a servidores: o pedido final é
> montado no dispositivo e enviado pelo WhatsApp da loja.

---

## ✅ Requisitos atendidos

| Requisito | Como foi implementado |
|---|---|
| Tema e funcionalidades livres | Loja de tequeños com cardápio, carrinho e checkout |
| HTML, CSS e JavaScript | Projeto 100% vanilla (sem dependências, sem build) |
| Interface **mobile-first** | CSS baseado em telas pequenas, com media queries `min-width` para tablets/desktop |
| Usabilidade mobile | Alvos de toque ≥ 44 px, HUD inferior (tab bar), FAB do carrinho, feedback com toasts, máscaras de input, `safe-area` para iPhone |
| `manifest.json` | Nome, ícones (192/512 + maskable), `start_url`, `display: standalone`, tema, atalhos (shortcuts) e screenshots |
| Service worker (`sw.js`) | Pré-cache do app shell, *stale-while-revalidate* para assets e fallback offline para navegações |
| Instalável como PWA | HTTPS no Render + manifest + service worker completam os critérios do Chrome/Android e iOS ("Adicionar à Tela de Início") |

Extras: modo offline com banner de status, modo escuro, cupons (`TEQUEIRO10`,
`FRETEGRATIS`), frete grátis acima de R$ 60, histórico de pedidos com "pedir de novo".

---

## 🗂 Estrutura

```
.
├── index.html              # página única (SPA de 1 arquivo)
├── manifest.json           # manifest da PWA (exigência do exercício)
├── sw.js                   # service worker (cache offline)
├── render.yaml             # blueprint de deploy no Render.com
└── assets/
    ├── css/styles.css      # estilos com design tokens + dark mode
    ├── js/app.js           # estado, carrinho, checkout, PWA
    └── icons/              # ícones gerados (PNG/SVG)
```

---

## 🚀 Rodando localmente

Qualquer servidor estático serve (não abra o `index.html` via `file://`,
senão o service worker não registra):

```bash
python3 -m http.server 8080
# abra http://localhost:8080
```

`localhost` é considerado contexto seguro, então a instalação e o service worker
funcionam para testes.

---

## ☁️ Publicando no Render.com via GitHub

### 1. Subir para o GitHub

```bash
git init -b main
git add .
git commit -m "feat: PWA Tequeño Loco"
```

Crie um repositório em https://github.com/new (nome sugerido: `tequeno-loco`,
**público** facilita a integração) e:

```bash
git remote add origin https://github.com/SEU_USUARIO/tequeno-loco.git
git push -u origin main
```

Com a CLI do GitHub (opcional): `gh repo create tequeno-loco --public --source=. --push`
requires `gh auth login` antes.

### 2. Criar o serviço no Render

Opção A — **Blueprint** (usa o `render.yaml` deste repositório):

1. Acesse https://dashboard.render.com → **New +** → **Blueprint**
2. Conecte o repositório `tequeno-loco` → **Apply**
3. Pronto: a URL tipo `https://tequeno-loco.onrender.com` é gerada

Opção B — **manual**:

1. **New +** → **Static Site**
2. Conecte o GitHub e selecione o repositório
3. **Name:** `tequeno-loco` · **Branch:** `main`
4. **Build Command:** *vazio* (site estático, sem build)
5. **Publish Directory:** `.` (a raiz)
6. **Create Static Site**

> O Render fornece HTTPS automaticamente — requisito obrigatório para PWA.

### 3. Validar

- Chrome DevTools → *Application* → *Manifest*: sem erros e "installable"
- *Application* → *Service Workers*: `sw.js` ativado
- *Lighthouse* → categorias *Performance* + *Best Practices* + *PWA*
- No Android: o Chrome oferece **"Instalar app"**; no iOS: Compartilhar →
  **"Adicionar à Tela de Início"**
- Depois da 1ª visita, desligue o Wi-Fi e recarregue: o app continua funcionando

---

## 🎨 Personalização rápida

| O quê | Onde |
|---|---|
| Número do WhatsApp da loja | `CONFIG.whatsapp` em `assets/js/app.js` (também no rodapé do `index.html`) |
| Taxa de entrega / frete grátis | `CONFIG.deliveryFee`, `CONFIG.freeDeliveryFrom` |
| Cupons | `CONFIG.coupons` |
| Cardápio ( itens, preços, badges) | array `PRODUCTS` em `assets/js/app.js` |
| Cores e dark mode | tokens em `:root` / `[data-theme="dark"]` de `assets/css/styles.css` |
| Ícones | `assets/icons/` (substitua pelos oficiais em 192/512 px) |

Feito para estudo — ajuste, quebre, refaça. Queijo é sempre bem-vindo. 🧀
