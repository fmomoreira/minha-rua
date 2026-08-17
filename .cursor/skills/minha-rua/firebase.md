# Firebase — MVP Spark

Plano **Spark (gratuito)**. Sem Cloud Functions. Sem Blaze.

## Serviços

| Serviço | Uso |
|---|---|
| Firestore | coleção `imoveis` |
| Storage | `imoveis/{id}/fachada.jpg` |
| Hosting | site estático |

Config no cliente: `js/config.js` com `apiKey`, `authDomain`, `projectId`,
`storageBucket`, `messagingSenderId`, `appId`. Não inventar credenciais.
Se o arquivo estiver vazio, o app deve avisar e funcionar em modo demo local.

## Schema `imoveis`

```js
{
  resident: string,
  street: string,
  number: string,
  neighborhood: string,
  complement: string,
  cep: string,
  city: string,          // "São José do Belmonte"
  state: string,         // "PE"
  lat: number,
  lng: number,
  photoUrl: string,
  contact: string,
  searchText: string,
  createdAt: Timestamp,
  public: true
}
```

Normalizar `searchText` no cliente antes do `addDoc`:

```js
function norm(s) {
  return String(s || '').toLowerCase()
    .normalize('NFD').replace(/[\u0300-\u036f]/g, '')
    .replace(/[^a-z0-9.\- ]+/g, ' ').trim();
}
```

Incluir `lat` e `lng` (string) dentro de `searchText`.

## Busca

No MVP: `getDocs(collection(db, 'imoveis'))` e filtro

`house.searchText.includes(norm(query))`.

Não criar índices compostos agora. Volume esperado: dezenas/centenas de casas.

## Regras Firestore

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /imoveis/{id} {
      allow read: if true;
      allow create: if validHouse();
      allow update, delete: if false;
    }
  }
}
```

`validHouse()` exige `street`, `neighborhood`, `lat`, `lng`, `photoUrl`,
`public == true`, `lat`/`lng` number, strings com tamanho máximo razoável
(rua ≤ 120, foto URL https).

## Regras Storage

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /imoveis/{id}/fachada.jpg {
      allow read: if true;
      allow write: if request.resource.size < 3 * 1024 * 1024
                   && request.resource.contentType.matches('image/.*');
    }
  }
}
```

## Fluxo de cadastro

1. Comprimir a foto no cliente (max ~1600px, JPEG).
2. `addDoc` com `photoUrl` temporário vazio **ou** gerar id, upload, depois `addDoc` com URL final.
   Preferir: `doc(collection).id` → upload → `setDoc` com `photoUrl`.
3. Geocodificar com Nominatim se lat/lng vazios.
4. Recusar submit sem rua, bairro e par lat/lng.

## Geo (gratuito)

- ViaCEP: `https://viacep.com.br/ws/{cep}/json/`
- Nominatim: `https://nominatim.openstreetmap.org/search?format=json&q=`
  + header `Accept-Language: pt-BR`. Respeitar 1 req/s.
- Centro do mapa default: **-7.863, -38.760** (São José do Belmonte).
- Leaflet tiles: `https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png`

## O que não fazer

- Não ligar Authentication no MVP.
- Não usar Google Maps Platform (chave paga).
- Não criar Functions.
- Não commitar `.env` com secrets além da config web pública do Firebase.
