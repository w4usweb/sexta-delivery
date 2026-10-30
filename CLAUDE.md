# Sexta Delivery — contexto do projeto

Site institucional/cardápio do Sexta Delivery (Pato Branco/PR, Dois Vizinhos/PR, São Lourenço do Oeste/SC).
Idioma do site e das conversas: português do Brasil.

## Tecnologia
- Site estático: HTML + CSS + JavaScript puro (sem framework, sem bundler, sem `package.json`, sem dependências).
- `dist/index.html` — marcação de todas as seções e dos `<dialog>` (cidade, produto, sacola, info).
- `dist/app.js` — toda a lógica; dados embutidos no topo (`stores`, `products`, `extraMenu`). Renderiza cardápio,
  lojas, sacola; busca e filtro por categoria; galeria de ângulos; formulário de expansão.
- `dist/style.css` — CSS original minificado (layout e estrutura). Breakpoints: 1500px, 1100px, 760px;
  respeita `prefers-reduced-motion`.
- `dist/theme.css` — identidade visual (carregado depois do style.css, sobrescreve cores, tipografia e
  acabamento). É aqui que se mexe no visual. Ver "Identidade visual" abaixo.
- Fontes: Google Fonts via `@import` (Big Shoulders Display/Text em theme.css; DM Sans em style.css).
- `dist/assets/` — fotos de produtos (JPG otimizado), `favicon.svg` e `brand/` (lettering e logo do Pub).
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

## Identidade visual (alinhada ao SextaBeer Pub, repositório `w4usweb/sextabeer-site`)
- Base híbrida: header, hero, unidades e footer no "palco" escuro do Pub (`--stage #15100d`, texto
  `--on-dark #ecddbf`); combos, cardápio, seção humana, expansão, FAQ e diálogos em papel (`--paper #ecddbf`,
  cards `--paper-card #f6eee0`, texto `--ink #1a1310`, secundário `--muted #5e4e3b`).
- Amarelo unificado com o Pub: `--yellow #f4b41a`. Laranja `--orange` para rótulos no escuro, `--rust` no papel.
- NUNCA amarelo como texto sobre papel (1,4:1). No papel, destaque de título (`h2 em`) é contorno escuro.
- Tipografia: Big Shoulders Display (títulos e preços, maiúsculas), Big Shoulders Text (rótulos/botões,
  letras espaçadas), DM Sans (textos corridos e descrições — legibilidade no celular).
- Marca: lettering "Sexta" recortado do logo do Pub + "DELIVERY" (`assets/brand/sexta-lettering-*.png`);
  logo "Sexta Beer pub" na faixa do pub (`assets/brand/sextabeer-pub-creme.png`). Textos originais seguem no HTML
  para leitores de tela.
- Detalhes do Pub: botões principais com recorte de ingresso (`--ticket`), foto "impressa" inclinada com fita,
  grão (`--grain`) nas superfícies escuras, cantos retos.
- Contraste conferido: todos os textos ≥ 4,5:1 (grandes ≥ 3:1). Foco: contorno escuro no papel, amarelo no escuro.

## Fotos
- Foto real do Sexta (vinda de `fotos/` do repositório do Pub, uso autorizado): só o Onion Rings Burger
  (`burger-onion.jpg`). Produtos com foto real têm `real: true` em `app.js` (legenda "Foto real").
- Combos Família e Casal usam as imagens originais do Delivery (geradas com IA): o dono preferiu essas às
  fotos reais do Pub. Não trocar sem pedido. Combo Individual fica com a ilustração "NA CHAPA.".
- Também geradas com IA: pizza, Bacon Burguer (`burger.jpg`), batata e entregador.
- Só associar foto a produto quando ela mostrar ESSE produto. `comida4 (porção)` do Pub não foi usada
  (provável tilápia, não confirmado).

## Cardápio
- Grade principal: itens de `products` sem `compact` (pizza, 6 burgers, batata, refri 2L, água).
- "Mais opções no cardápio" (`<details id="more-menu">`): itens de `extraMenu` (`compact: true`), em lista.
  Busca e filtro valem para os dois; a lista some quando não há itens e abre sozinha quando a busca encontra algo.
- Produto sem foto (`images: []`) mostra ilustração: garrafa para bebidas, "NA CHAPA." para burgers,
  "PRA DIVIDIR." para porções (`productGraphic` em `app.js`).

## Problemas conhecidos
- Burgers Salada, Tradicional, Kids e Combo Individual sem foto (ilustração "NA CHAPA.").
- Página inteira ≈ 1 MB (antes 2 MB). Imagens ainda em JPG, não WebP/AVIF (o `app.js` monta `assets/<id>.jpg`).
- "Combo Individual" está sem preço (divergência no cardápio de origem).

## Ao fazer alterações
- Editar direto os arquivos em `dist/`; não há etapa de build.
- Testar localmente com `node server.cjs` antes de enviar.
