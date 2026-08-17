---
name: evoluir-mvp
description: >-
  Fluxo para evoluir o protótipo Minha Rua em MVP com Firebase: extrair UI,
  cadastrar imóvel público, mapa, lat/long e busca. Use quando o usuário pedir
  para evoluir, implementar, cadastrar imóvel, conectar Firebase ou sair do
  protótipo.
---

# Evoluir o MVP

Leia [docs/tecnica.md](../../../docs/tecnica.md) e o skill `minha-rua` antes
de escrever código.

## Ordem obrigatória

Não pule etapas. Não introduza React, Vite, login ou Functions.

```
Progresso:
- [ ] 1. Extrair o protótipo para HTML/CSS/JS estáticos
- [ ] 2. Ampliar o cadastro (bairro, CEP, lat, long, mapa)
- [ ] 3. Conectar Firestore + Storage
- [ ] 4. Ligar a busca a searchText
- [ ] 5. Regras Firebase + firebase.json
```

## Etapa 1 — Extrair UI

Substituir o `index.html` bundle por um SPA hash estático com as 5 views
do protótipo (home, resultados, casa, cadastrar, sucesso).

Manter tokens, copy e SVGs. Dados ainda podem ser um array local nesta etapa.

## Etapa 2 — Endereço completo

No formulário, gravar `street`, `neighborhood`, `lat`, `lng`.
ViaCEP no blur do CEP. Nominatim se o mapa estiver vazio.
Pino arrastável atualiza lat/long. Sem esses três campos, não submete.

## Etapa 3 — Firebase

`js/config.js` com placeholders. Upload da foto ≤ 3 MB.
`setDoc` com `searchText` e `public: true`. Sem update/delete.

## Etapa 4 — Busca

Uma query string. `norm(q)` ⊆ `house.searchText`.
Vazio = listar todas. Chips = a mesma função.

## Etapa 5 — Regras

Criar `firestore.rules`, `storage.rules`, `firebase.json` (hosting `.`).
Documentar no README só o mínimo: criar projeto Spark, colar config, deploy.

## Pronto quando

- Casa nova aparece para outra aba/browser
- Foto pública
- Busca por rua, bairro ou pedaço de lat/long funciona
- Ficha abre Waze/Maps no ponto certo
