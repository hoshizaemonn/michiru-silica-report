# Imagen プロンプト集 — 四谷サイネージ 15秒広告 v1

**生成方法:** `get_api_key("michiru")` でAPI Key取得 → Imagen 4.0 / Imagen 2 で生成
**アスペクト比:** 16:9（横型サイネージ）
**重要:** 日本語テロップは後付け（既存 text_renderer.py 使用）。**画像内にテキストは入れない**

---

## C1 / 0-2s — 理想の姿（窓辺の女性）

**Positive Prompt（EN）:**
```
A serene morning scene of an elegant 55-year-old Japanese woman in a soft beige cashmere cardigan, standing by a large window holding a clear glass of water in both hands, gentle sidelight casting warm golden tones across her face and shoulders, her expression peaceful with a slight smile, backlit silhouette on the right third of the frame, soft window light bokeh on the left, minimalist upscale living room, warm cream and amber color palette, premium lifestyle photography, cinematic 16:9, shallow depth of field, Hasselblad 80mm, ultra-realistic skin texture, no text, no logo
```

**Negative:** `text, letters, logo, watermark, harsh shadows, cartoon, young woman, casual clothing, plastic bottle`

**配置メモ:** 人物は右1/3、左に明るい余白＋窓光。テロップ「飲むだけで、内側から。」は左に重ねる。

---

## C2 / 2-5s — 共感（失われるシリカ）

**Positive Prompt（EN）:**
```
Abstract dark cinematic scene of luminous golden sand particles slowly falling vertically in a thin stream against deep navy and charcoal background, particles glowing with soft amber light, faint scientific element symbol "Si" softly embossed in the dark background on the left side as subtle texture, ethereal floating dust motes, dramatic side lighting, macro photography aesthetic, premium pharmaceutical commercial style, 16:9, cinematic depth, blurred bokeh background, no text overlay, no Japanese characters
```

**Negative:** `text, characters, hourglass shape, sand on ground, bright background, colorful, cartoon`

**配置メモ:** 砂粒モーションはVeo 2で横パン or Veo 3 Fastで微速度。「Si」は背景の質感扱いなので画像生成OK、目立ちすぎたらPhotoshopで薄める。

---

## C3 / 5-8s — 解決（ミチル実ボトル＋霧島背景）

**【方針】** 実ボトル写真（PET透明・グレーキャップ・青ラベル・シリカ72mgバッジ付き）を切り抜いて合成する前提で、**Imagenでは背景のみ生成**する。ボトルそのものは生成しない（商標・形状の再現性確保のため）。

**Positive Prompt（EN・背景のみ）:**
```
A breathtaking landscape of Kirishima volcanic mountain range in southern Japan, shrouded in soft morning fog and ethereal mist, layered mountain silhouettes fading into pale blue distance, golden sunrise light glowing softly at the horizon line on the right, cool teal and silver-grey atmospheric haze, dramatic cloud inversion rolling between peaks, serene and pristine natural scene, premium beverage commercial background plate, completely empty composition with no objects in the foreground, the left third of the frame intentionally clear and atmospheric for product placement, 16:9 widescreen cinematic, ultra-detailed misty atmosphere, soft natural daylight, no bottles, no products, no text, no people
```

**Negative:** `bottles, products, objects in foreground, text, logos, people, harsh sunlight, bright daytime sky, colorful elements`

**合成手順:**
1. このプロンプトで霧島背景プレートを生成
2. 提供されたミチル実ボトル画像（PET透明・グレーキャップ・青ラベル・「シリカ 72mg」オレンジバッジ）を切り抜き
3. 左1/3に配置、わずかにドロップシャドウ＋リムライト追加
4. Veo 2でボトルにゆっくり横パン or ズーム

---

## C4 / 8-12s — 根拠（グラスに注ぐ）

**Positive Prompt（EN）:**
```
Cinematic slow-motion macro shot of crystal-clear mineral water pouring into an elegant tall glass on the right side of the frame, water cascading in a smooth ribbon, air bubbles suspended mid-fall, splash droplets frozen in motion, dark navy and deep teal background with soft blue rim light, the glass catching subtle highlights, ultra-high-speed photography aesthetic, premium luxury water commercial, dramatic side lighting, water surface reflecting cool blue tones, left third of frame intentionally empty with deep dark space for typography, 16:9 cinematic, hyper-realistic water physics, no text, no logo
```

**Negative:** `text, numbers, characters, ice cubes, fruit, lemon, colored water, bright background, daytime`

**配置メモ:** 左1/3を意図的に空けて「72mg/L」の大型数値テロップを乗せる。Veo 3 FastでスローモーションV化推奨。

**重要:** 実ボトルのラベル記載は「シリカ 72mg」。誤って「97mg/L」と表記しないこと。

---

## C5 / 12-15s — CTA（指名検索）

**Positive Prompt（EN）:**
```
Minimalist elegant beige and cream textured paper background with subtle warm gradient, soft natural light from upper left, very slight grain texture like premium washi Japanese paper, completely empty composition with no objects, gentle vignetting at corners, calm and trustworthy atmosphere, premium brand aesthetic, neutral warm color palette (#f5f0e8 to #e8ddc8), photography studio backdrop style, 16:9 widescreen, ultra-clean composition for typography overlay, no text, no graphics, no objects
```

**Negative:** `text, logo, objects, people, products, decorative elements, dark colors, complex patterns`

**配置メモ:** これは完全に「テロップ＋検索ボックスUIを乗せるための背景」。シンプルな上品な紙質感のみ。検索ボックスUIはSVGで重ねる（既に storyboard_v1.html に実装済みデザインを書き出して使用）。

---

## 生成コマンド例（既存パイプライン準拠）

```python
import sys
sys.path.insert(0, "/Users/hoshizaki/-hishaku-/受託事業/_共通")
from api_cost_logger import log_cost, get_api_key

API_KEY = get_api_key("michiru")
# 各カット 4枚生成 → ベストショット選定
# c1.png / c2.png / c3.png / c4.png / c5.png
```

**生成後の工程:**
1. ベストショット選定（各カット4枚生成→1枚採用）
2. video-generator パイプライン投入（Ken Burns or Veo動画化）
3. text_renderer.py でテロップ焼き込み（ハリナチュレ式・既存スタイル）
4. C5には検索ボックスUI SVG オーバーレイ
5. 合成 → 15秒MP4出力

---

## チェックリスト

- [ ] 全プロンプトに「no text, no logo」明記済み
- [ ] アスペクト比 16:9 統一
- [ ] 富裕層トーン（明朝感・上品な配色）を全カットで指定
- [ ] C3のボトルは別素材合成前提（薬機・商標リスク回避）
- [ ] C4は左1/3を空けて数値テロップ用スペース確保
- [ ] C5は背景のみ（UI要素は後付け）
