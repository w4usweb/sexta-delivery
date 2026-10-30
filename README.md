# Sexta Delivery

Site estático (HTML, CSS e JavaScript puro, sem build) do Sexta Delivery.

## Estrutura

- `dist/` — o site publicado (`index.html`, `style.css`, `app.js`, `assets/`)
- `server.cjs` — servidor local simples para testes
- `vercel.json` — configuração da Vercel (publica a pasta `dist`, sem etapa de build)

## Rodar localmente

```bash
node server.cjs
```

Abra http://127.0.0.1:4173

## Publicação

Cada `git push` na branch `main` gera uma nova publicação na Vercel automaticamente.
Não há variáveis de ambiente necessárias.
