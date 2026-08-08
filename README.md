# [跑道測試場 · Tinkercad Car Test Track](https://allenlin316.github.io/tinkercad-test-track/)

把你在 [Tinkercad](https://www.tinkercad.com/) 設計的車子匯出後，直接丟到這個網頁裡，就能在一條有直線、大彎、髮夾彎的跑道上開圈計時。純前端、單一 HTML 檔案，不需要任何後端或建置流程。

## 線上使用

打開 [`index.html`](./index.html) 即可（需透過 HTTP server 開啟，例如 GitHub Pages、`python -m http.server`，直接用 `file://` 開啟會因瀏覽器安全限制無法讀取預設車款檔案）。

開啟頁面後會停留在起始畫面，讓你選擇要「使用預設車款試跑」（載入 [`finished_cars/yellow_sport_car.glb`](./finished_cars/yellow_sport_car.glb)）還是拖曳／選擇自己的模型檔案；進入跑道後也能隨時按「換一台車」重新選檔。

## 功能

- **免安裝的模型解析器**：純手刻 GLB / glTF、STL、OBJ（可搭配 .mtl）解析邏輯，不依賴 three.js 官方 loader，單一 HTML 檔就能跑。
- **拖曳或選檔匯入**：支援拖放到畫面上，或用檔案選擇器匯入；`.glb` 會保留 Tinkercad 匯出的顏色。
- **自動貼合車輛**：匯入後自動置中、貼地、依車長縮放到固定比例，並判斷車頭朝向。
- **手動微調**：可用面板調整車身方向（±90°旋轉、直立修正）、大小縮放、是否套用自訂顏色。
- **兩種駕駛模式**：
  - 自動繞圈：沿著跑道中心線固定速度繞圈，可調整速度。
  - 手動駕駛：方向鍵 / WASD（手機則有虛擬方向鍵）操控油門、煞車與轉向。
- **三種攝影機視角**：跟車、空拍、環繞（可拖曳旋轉、滾輪縮放）。
- **跑圈計時 HUD**：即時時速、圈數、本圈時間、最佳單圈時間，並在偏離跑道時提示。
- **存成圖片**：一鍵把目前畫面存成 PNG。
- **中文介面**，操作提示皆為繁體中文。

## 如何從 Tinkercad 匯出車子

1. 在 Tinkercad 打開你的設計。
2. 右上角 **Export → .glTF (.glb)**（建議用 `.glb`，單一檔案且保留顏色）。
3. 把匯出的檔案拖進網頁，或用「換一台車」重新選檔。

也支援 `.stl`（無顏色）與 `.obj`（可一併選取對應的 `.mtl` 檔案）。

## 專案結構

```
index.html                          # 全部邏輯（解析器、場景、UI）都在這個檔案裡
finished_cars/yellow_sport_car.glb  # 預設示範車款
```

## 技術細節

- 使用 [three.js](https://threejs.org/)（透過 CDN 載入 r128）畫場景、跑道、光影與陰影。
- 跑道路徑用 `CatmullRomCurve3` 產生封閉曲線，並取樣算出路面帶狀網格、中線虛線、起跑線與周邊裝飾物（三角錐、樹木、建築物）。
- GLB/glTF、STL、OBJ 的二進位/文字格式皆為手動解析（含 accessor、bufferView、節點階層轉換矩陣運算等），未使用 `GLTFLoader` 等官方套件。
