# Sexta Delivery — contexto do projeto

Site institucional/cardápio do Sexta Delivery (Pato Branco/PR, Dois Vizinhos/PR, São Lourenço do Oeste/SC).
Idioma do site e das conversas: português do Brasil.

## Tecnologia
- Site estático: HTML + CSS + JavaScript puro (sem framework, sem bundler, sem `package.json`, sem dependências).
- `dist/index.html` — marcação de todas as seções e dos `<dialog>` (cidade, produto, sacola, info).
- `dist/app.js` — toda a lógica; dados embutidos no topo (`stores`, `products`, `extraMenu`). Renderiza cardápio,
  lojas, sacola; busca e filtro por categoria; galeria de ângulos; formulário de expansão.
- `dist/style.css` — CSS minificado (11 linhas longas). Variáveis em `:root` (`--ink`, `--cream`, `--yellow`,
  `--muted`, `--line`, `--font`, `--display`). Breakpoints: 1500px, 1100px, 760px; respeita `prefers-reduced-motion`.
- Fontes: Google Fonts via `@import` no CSS (Barlow Condensed + DM Sans). Única dependência externa.
- `dist/assets/` — 7 JPGs 1536×1024 (≈220–480 KB cada, geradas com IA) + `favicon.svg`.
- `server.cjs` — servidor Node local só para testes: `node server.cjs` → http://127.0.0.1:4173

## Como o site funciona
- Sem back-end, sem banco, sem pagamento, sem login. Nenhuma variável de ambiente ou segredo.
- Cidade escolhida e sacola ficam no `localStorage` (`sexta-city`, `sexta-bag`).
- "Pedido" = mensagem montada e aberta no WhatsApp da unidade (`https://wa.me/<phone>`). Pato Branco também
  tem link para o Brendi (`pedido.brendi.com.br/sextabeer`). Formulário de expansão abre WhatsApp de Pato Branco.
- Telefones, Instagram e endereços das lojas estão no array `stores` em `dist/app.js` (dados públicos).
- Observações do cliente são escapadas com `escapeHtml` antes de ir ao DOM; dados de produto são estáticos.

## Produção
- GitHub: https://github.com/w4usweb/sexta-delivery (branch `main`).
- Vercel: projeto `sexta-delivery` no time `w4uswebs-projects` (plano Hobby), ligado ao GitHub.
  Todo push em `main` publica em produção; outras branches geram preview.
- `vercel.json`: `framework: null`, `outputDirectory: "dist"` — sem install e sem build.
- A pasta `.openai/` (hospedagem anterior) fica fora do Git via `.gitignore`.

## Cardápio
- Grade principal: itens de `products` sem `compact` (pizza, 6 burgers, batata, refri 2L, água).
- "Mais opções no cardápio" (`<details id="more-menu">`): itens de `extraMenu` (`compact: true`), em lista.
  Busca e filtro valem para os dois; a lista some quando não há itens e abre sozinha quando a busca encontra algo.
- Produto sem foto (`images: []`) mostra ilustração: garrafa para bebidas, "NA CHAPA." para burgers,
  "PRA DIVIDIR." para porções (`productGraphic` em `app.js`).

## Problemas conhecidos
- Burgers Salada, Tradicional, Onion Rings, Kids e Combo Individual ainda sem foto; Bacon usa foto gerada com IA.
  A rede da sessão de nuvem bloqueia bancos de imagem (Unsplash, Pexels etc.).
- Imagens sem versão otimizada (WebP/tamanhos menores); página carrega ~0,85–1 MB.
- "Combo Individual" está sem preço (divergência no cardápio de origem).

## Ao fazer alterações
- Editar direto os arquivos em `dist/`; não há etapa de build.
- Testar localmente com `node server.cjs` antes de enviar.
