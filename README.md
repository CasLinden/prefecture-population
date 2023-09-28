# 都道府県人口　 prefecture-population

[Hosted Live with Vercel](https://prefecture-population-d933v3h2r-caslindens-projects.vercel.app/)

## コンポーネント構造   Component Structure 

```
App.vue
│
├── SiteHeader.vue
│
└── PopulationFetcher.vue
    │
    ├── PopulationChart.vue
    │   │
    │   ├── PopulationTypeToggle.vue
    │   │
    │   └── Chart.js Canvas
    │
    └── AreaTrays.vue
        │
        └── CheckboxTray
```


Data from: [RESAS API](https://opendata.resas-portal.go.jp/docs/api/v1/index.html)

## このプロジェクトについて  Aout this project


このプロジェクトは、フロントエンドスキルを示すための課題として作成されました。RESAS APIからデータを取得し、日本の47都道府県の人口データの折れ線グラフをレスポンシブに表示します。[Vue.js](https://vuejs.org/)（プログレッシブJavaScriptフレームワーク）を使用して作成されました。チャートは[chart.js](https://www.chartjs.org/)を用いて生成されています。

This project was made as an assignment to demonstrate front-end skills. It fetches data from the RESAS API, to responsively display line graphs of the population figures of Japan's 47 prefectures. Made with [Vue.js](https://vuejs.org/), the progressive JavaScript framework. The charts are generated with [chart.js](https://www.chartjs.org/).

