# Minha Rua — Documentação técnica

Fonte de verdade do produto e da arquitetura. Leia isto antes de alterar código.

## 1. O que é

Minha Rua é um diretório público de casas em **São José do Belmonte — PE**.

A pessoa acessa, cadastra a própria casa (foto + endereço completo) e qualquer outra pessoa encontra essa casa pesquisando por rua, bairro, número, morador, CEP ou coordenadas.

Sem login no MVP. Cadastro público. Firebase no plano gratuito (Spark).

## 2. Estado atual do repositório

| Item | Situação |
|---|---|
| Repo | `git@github.com:fmomoreira/minha-rua.git` |
| Branch | `main` |
| Código | Um único `index.html` (~314 KB) — protótipo empacotado (bundler) |
| Persistência | Nenhuma. Casas ficam só na memória do browser |
| Backend | Nenhum |
| Firebase | Não conectado |
| Hospedagem | Site estático (`minharua.com.br`) + `.well-known/acme-challenge` |

O `index.html` atual **não deve ser editado como fonte**. É um artefato gerado. A evolução substitui esse bundle por HTML/CSS/JS estáticos, mantendo a identidade visual.

### O que o protótipo já faz (só no cliente)

- Home com busca por rua, número ou nome do morador
- Chips de ruas populares
- Lista de resultados com foto, número, morador e WhatsApp
- Ficha da casa: endereço, Waze, Google Maps, WhatsApp, link compartilhável
- Formulário de cadastro: foto da fachada, rua, número, morador, WhatsApp
- Sem login, cidade fixa: São José do Belmonte · PE

### O que o protótipo **não** tem (e o MVP precisa)

- Persistência (Firebase)
- Foto real enviada para storage
- Bairro, CEP, complemento
- Latitude e longitude
- Busca por bairro / lat / long / endereço completo
- Mapa na ficha da casa
- URL real por casa (`#/casa/{id}`)

## 3. Decisão de produto do MVP

Alinhado com o pedido do produto:

1. Pessoa acessa e cadastra a casa.
2. Cadastro é **público**: foto + endereço completo.
3. Endereço mapeia **rua, bairro, lat e long**.
4. Busca encontra a casa por **qualquer um desses campos**.
5. Backend = **Firebase gratuito** neste primeiro momento.

### Fora do MVP

- Login / autenticação
- Edição ou exclusão pelo morador
- Moderação administrativa
- App nativo
- Pagamento
- Cloud Functions
- Busca full-text paga (Algolia, Typesense Cloud)

## 4. Arquitetura alvo

```
Browser (HTML/CSS/JS estático)
    │
    ├── Firestore  → documentos públicos da coleção `imoveis`
    ├── Storage    → fotos da fachada em `imoveis/{id}/fachada.jpg`
    └── Hosting    → site estático (plano Spark)
```

Sem servidor próprio. Sem build obrigatório. Firebase JS SDK via CDN.

Geocodificação e CEP usam APIs gratuitas no cliente:

- **ViaCEP** — preenche rua, bairro, cidade, UF a partir do CEP
- **Nominatim (OpenStreetMap)** — gera lat/long a partir do endereço
- **Geolocalização do browser** — atalho “usar minha localização”
- **Leaflet + OSM** — mapa na ficha e no cadastro (sem chave Google)

## 5. Modelo de dados

Coleção Firestore: `imoveis`

```js
{
  resident: string,        // nome do morador
  street: string,          // rua
  number: string,          // número (ou "s/n")
  neighborhood: string,    // bairro
  complement: string,      // opcional
  cep: string,             // 00000-000
  city: string,            // default "São José do Belmonte"
  state: string,           // default "PE"
  lat: number,             // latitude
  lng: number,             // longitude
  photoUrl: string,        // URL pública do Storage
  contact: string,         // WhatsApp / telefone
  searchText: string,      // campos normalizados concatenados
  createdAt: timestamp,
  public: true
}
```

`searchText` = versão minúscula, sem acento, de:

`resident + street + number + neighborhood + cep + city + state + lat + lng`

A busca do MVP filtra no cliente sobre `searchText` (volume pequeno). Isso cobre rua, bairro, lat e long sem índice composto.

ID do documento = gerado pelo Firestore. Slug de compartilhamento:

`{slug(street)}-{number}-{idCurto}`

## 6. Regras Firebase (Spark)

### Firestore

- Leitura pública da coleção `imoveis`
- Criação pública com validação de campos obrigatórios
- Sem update/delete no MVP (evita vandalismo fácil)

Campos obrigatórios na rule: `street`, `neighborhood`, `lat`, `lng`, `photoUrl`, `public == true`.

### Storage

- Leitura pública de `imoveis/{id}/**`
- Upload público só em `imoveis/{id}/fachada.jpg`
- Tipo `image/*`, tamanho máximo 3 MB

### Custo (Spark, suficiente para o MVP)

- Firestore: 50 mil leituras / 20 mil escritas / dia
- Storage: 5 GB + 1 GB download / dia
- Hosting: 10 GB storage + 360 MB/dia

Não usar Cloud Functions no Spark (requer Blaze).

## 7. Telas

1. **Home** — busca + chips + contador de casas
2. **Resultados** — lista filtrada
3. **Casa** — foto, endereço completo, mapa, lat/long, Waze, Maps, WhatsApp, link
4. **Cadastrar** — foto, CEP, rua, número, bairro, complemento, lat/long (auto + ajuste no mapa), morador, WhatsApp
5. **Sucesso** — link compartilhável

Rotas em hash para permanecer estático:

- `#/`
- `#/buscar?q=`
- `#/casa/{id}`
- `#/cadastrar`

## 8. Identidade visual (não mudar)

| Token | Valor |
|---|---|
| Primária | `#cf5a28` |
| Primária escura | `#b0491d` |
| Primária suave | `#f8e7db` |
| Verde | `#1f8f5a` |
| Fundo | `#faf5ee` |
| Card | `#ffffff` |
| Texto | `#2b221c` |
| Mudo | `#8b7d71` |
| Linha | `#ece0d1` |
| Título | Bricolage Grotesque 700–800 |
| Corpo | Plus Jakarta Sans 400–700 |

Tom: simples, local, sem jargão. Cidade âncora: São José do Belmonte · PE.

## 9. Estrutura de pastas alvo (quando evoluir)

```
index.html
css/app.css
js/config.js          # credenciais Firebase (não commitar segredos reais se houver)
js/firebase.js
js/search.js
js/geo.js
js/app.js
docs/tecnica.md
firebase.json
firestore.rules
storage.rules
.cursor/skills/
.cursor/rules/
```

## 10. Como evoluir (ordem)

1. Extrair UI do protótipo para HTML/CSS/JS estáticos (mesma cara).
2. Adicionar campos de endereço completo + mapa no cadastro.
3. Conectar Firestore + Storage.
4. Ligar busca aos campos rua, bairro, lat, long e texto livre.
5. Publicar regras e hosting no Firebase Spark.

Não reescrever em React/Vite neste momento. O MVP precisa ser estático, barato e fácil de hospedar.
