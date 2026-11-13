# Boletim do Monitor da Água — MapBiomas Água

**Página:** https://pedrogalve00.github.io/boletim-monitor-agua/

Página única (`index.html`), no mesmo modelo do boletim do Monitor do Fogo: não tem build nem servidor.
Ao abrir, ela baixa as planilhas mensais v3 e os contornos (série considerada **a partir de 2019**, `ANO_INICIO` no código), acha sozinha o último mês com dados, faz as
contas no navegador e monta o boletim em slides 16:9, no visual do factsheet do MapBiomas Água
(Montserrat, logo no cabeçalho, número da página, barra azul). **Exportar PDF** usa a impressão do navegador.


## Estrutura

```
index.html                          página do boletim
img/logo_mapbiomas_agua.png         logo do cabeçalho (troque pelo arquivo oficial, se quiser)
dados/                              planilhas mensais v3 (trocar todo mês)
  biome-water_surface_monthly_v3.csv
  state-water_surface_monthly_v3.csv
  hydrographicRegion-water_surface_monthly_v3.csv
  subBasinPnrh1-water_surface_monthly_v3.csv
geo/                                contornos simplificados (gerar uma vez)
  bioma.geojson  estado.geojson  hidro.geojson  subbacia1.geojson
colab/exportar_contornos_boletim.ipynb   gera a pasta geo/ a partir dos assets do Earth Engine
```

## Uma vez: gerar os contornos

1. Abra `colab/exportar_contornos_boletim.ipynb` no Colab → Executar tudo (projeto `pedrogalve`).
2. Ele baixa `projects/mapbiomas-agua/assets/territories/{biome, state, hydrographicRegion, subBasinPnrh1}`,
   simplifica, deixa só `code` e `name`, arredonda as coordenadas e salva em `Meu Drive/dados/geo/`.
3. Para cada nível, ele diz quantos territórios casam com a planilha mensal (ex.: `27/27`) e o tamanho do arquivo.
4. Copie a pasta `geo/` para o repositório.

## Todo mês

1. Substitua os 4 CSVs em `dados/` (mesmos nomes), **só com os anos de 2019 em diante** — a página ignora anos anteriores, mas o que está no repositório fica público.
2. Publique (commit no repositório com GitHub Pages ligado).
3. Abra a página → **Exportar PDF**.

> Se o último mês da planilha só repetir o mês anterior (ex.: dezembro = novembro na maioria dos biomas), a página abre no mês anterior e mostra um aviso no topo. O mês repetido continua disponível no seletor.

## Slides

| | Conteúdo |
|---|---|
| 1 | Capa |
| 2 | Brasil (soma dos biomas): água do mês, triângulos vs média do mês, vs mês anterior e vs ano anterior, linhas mensais por ano (ano do boletim em azul-escuro, média em laranja), série do mês ao longo dos anos, posição no histórico |
| 3 | Biomas: mapa do desvio, ranking da variação absoluta, anel com % abaixo da média, caixas do maior ganho e da maior perda (com posição no mês e no ranking histórico) |
| 4 | Superfície de água mensal nos 6 biomas + destaques automáticos |
| 5 | UFs: mapa com siglas, ranking das 27 UFs, anel e caixas |
| 6 | Regiões hidrográficas: mapa, ranking de ganhos e perdas, anel e caixas |
| 7 | Bacias nível 1 (PNRH): mapa, 10 maiores ganhos e 10 maiores perdas, anel e caixas |
| 8 | Ranking histórico do Brasil: posição do mês entre todos os meses da série e os 20 meses de maior perda e de maior ganho |

Nos slides de Biomas, UFs, Regiões hidrográficas e Bacias, as caixas da direita destacam o **maior ganho e a maior perda absolutos (ha)** e o maior aumento/redução em relação ao mês anterior; ao lado do mapa ficam os destaques **relativos (%)**. Há também uma mensagem sob o mapa (onde estão as maiores perdas e ganhos) e sob o gráfico (quantos ficaram acima/abaixo da média, saldo e comparação com o mês anterior); as caixas de destaque também mostram a variação em relação ao mês anterior.

Cada gráfico e mapa tem botões **PNG** (fundo transparente, 4× a tela) e **CSV**. **Baixar dados (CSV)** traz
todos os territórios. As contas são as mesmas do Colab `boletim_agua_mensal.ipynb`.

## Endereços úteis

- `index.html?mes=2025-06` → um mês específico (também no seletor do topo)
- `index.html?dados=https://.../&geo=https://.../` → ler de outras pastas públicas
