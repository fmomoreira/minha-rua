# UI — Minha Rua

Reproduzir o protótipo. Não redesenhar.

## Tokens

```css
:root {
  --pri: #cf5a28;
  --pri-deep: #b0491d;
  --pri-soft: #f8e7db;
  --grn: #1f8f5a;
  --grn-soft: #e2f0e7;
  --bg: #faf5ee;
  --card: #ffffff;
  --ink: #2b221c;
  --mut: #8b7d71;
  --line: #ece0d1;
}
```

Fontes Google: **Bricolage Grotesque** (títulos 700–800) e
**Plus Jakarta Sans** (corpo 400–700).

## Peças que já existem

- Pin-casa SVG (gradiente `#e8825a` → `#c14e1f`, casa branca)
- Barra de busca branca, raio 18px, botão terracota
- Chips de rua (`border-radius: 20px`)
- Card de resultado: thumb 118×96, badge `Nº` terracota
- Foto vazia: listras `#efe4d6` / `#e7dac9`
- Botão Waze verde, Maps terracota
- Toast escuro centralizado embaixo
- Animations: `pop`, `toastIn`, `float`

## Home

Logo pin + título “Minha Rua” 62px. Subtítulo 16.5px mudo.
Placeholder da busca: “Digite a rua, o bairro, o número ou o morador...”
(atualizar o placeholder antigo para incluir bairro).
Contador: “N casas cadastradas em São José do Belmonte”.

## Cadastro (novos campos, mesmo estilo de input)

Inputs: borda `--line`, raio 12px, padding 13×15, fundo `--card`.
Depois de rua/número, nesta ordem:

1. CEP (preenche rua/bairro)
2. Bairro
3. Complemento
4. Mapa Leaflet (~180px) com pino arrastável + lat/long readonly
5. Botão “Usar minha localização”

## Responsivo

Mobile first. App bar empilha busca abaixo do logo em telas < 720px.
Grid Waze/Maps vira 1 coluna no mobile.

## Acessibilidade mínima

Labels visíveis. `input[type=file]` associado à área da foto.
Foco visível nos inputs. Não depender só de cor no mapa.
