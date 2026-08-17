# Produto — Minha Rua

## Frase do produto

A pessoa acessa, cadastra a casa no Minha Rua, e o cadastro fica público com
foto e endereço completo (rua, bairro, lat e long) para qualquer busca.

## Personas

- **Morador** — quer ser encontrado. Cadastra em menos de 1 minuto, sem conta.
- **Visitante** — busca rua, bairro ou ponto no mapa e abre Waze/Maps/WhatsApp.

## Campos do cadastro

| Campo | Obrigatório | Origem |
|---|---|---|
| Foto da fachada | sim | upload → Storage |
| CEP | não | ViaCEP preenche o resto |
| Rua | sim | digitado ou ViaCEP |
| Número | sim | digitado (`s/n` se vazio) |
| Bairro | sim | digitado ou ViaCEP |
| Complemento | não | digitado |
| Cidade / UF | sim | default Belmonte / PE |
| Lat / Long | sim | Nominatim, GPS ou pino no mapa |
| Nome do morador | não | default `Morador(a)` |
| WhatsApp | não | default vazio (esconde botão) |

## Busca

Uma caixa. O termo casa em `searchText`. Exemplos válidos:

- `Rua da Matriz`
- `Centro`
- `45`
- `Marlene`
- `-7.86`
- `56950`

Chips da home (ruas populares) só disparam a mesma busca.

## Telas e rotas

| Rota | Tela |
|---|---|
| `#/` | Home |
| `#/buscar?q=` | Resultados |
| `#/casa/{id}` | Ficha pública |
| `#/cadastrar` | Formulário + sucesso |

Ficha da casa mostra: foto, morador, endereço completo, bairro, lat/long,
mapa Leaflet, Waze, Google Maps, WhatsApp, copiar link.

## Copy que já existe (manter o tom)

- “Ache e cadastre casas pelo endereço. Sem login, é só digitar a rua.”
- “Sem login. Leva menos de 1 minuto e ajuda todo mundo a te achar.”
- “Casa cadastrada! Agora é só compartilhar o link…”
- “Toque para adicionar a foto”
- “Nenhuma casa encontrada / Que tal cadastrar essa casa agora?”

## Fora do MVP

Login, edição, exclusão, moderação, app nativo, pagamento, Functions.
