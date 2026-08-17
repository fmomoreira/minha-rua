---
name: minha-rua
description: >-
  Contexto do produto Minha Rua (São José do Belmonte — PE): cadastro público
  de casas com foto, endereço completo, rua, bairro, lat/long, busca e Firebase
  Spark. Use em qualquer tarefa deste repositório, ao cadastrar imóvel, buscar
  casa, conectar Firebase, ou evoluir o MVP.
---

# Minha Rua

Leia [docs/tecnica.md](../../../docs/tecnica.md) antes de mudar código.

## Produto

Diretório **público** de casas. Sem login. A pessoa cadastra a casa; qualquer
pessoa encontra por rua, bairro, número, morador, CEP, lat ou long.

Cidade âncora: **São José do Belmonte · PE**.

## Estado atual

- Só existe `index.html` empacotado (protótipo). Casas **não** persistem.
- Não edite o bundle como fonte. A evolução cria HTML/CSS/JS estáticos.
- Firebase ainda não está conectado.

## Regras ao evoluir

1. Manter a identidade visual (terracota `#cf5a28`, fundo `#faf5ee`).
2. Cadastro público: foto da fachada + endereço completo.
3. Mapear **rua, bairro, lat e long** em todo imóvel.
4. Busca deve achar por qualquer um desses campos.
5. Backend = Firebase **Spark** (Firestore + Storage + Hosting). Sem Functions.
6. Sem login, sem React/Vite, sem servidor próprio no MVP.

## Modelo mínimo

```js
{
  resident, street, number, neighborhood, complement,
  cep, city, state, lat, lng, photoUrl, contact,
  searchText, createdAt, public: true
}
```

`searchText` = texto normalizado (minúsculo, sem acento) de todos os campos
pesquisáveis, inclusive `lat` e `lng`. Filtro no cliente no MVP.

## Referências

- Produto e telas: [produto.md](produto.md)
- Firebase, regras e busca: [firebase.md](firebase.md)
- UI e tokens: [ui.md](ui.md)
- Doc técnica completa: [docs/tecnica.md](../../../docs/tecnica.md)
