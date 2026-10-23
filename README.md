# Valorant Kill Icon Animation Preview / Valorant 击杀图标动画预览

A browser-based preview tool that reproduces the Valorant kill icon animation — a multi-layer compositing effect (frame / emblem / kill-count ring / halo ring / headshot marker / particles / flash) driven by sprite-sheet frame animations and eased transforms. No dependencies, no build step: open one HTML file and it works.

一个复现 Valorant 击杀图标动画的浏览器预览工具：多层合成（外框 / 徽章 / 击杀数 / 光环 / 爆头标记 / 粒子 / 闪光），由精灵图帧动画与缓动变换驱动。零依赖、无需构建，打开一个 HTML 文件即可使用。

- **Live demo / 在线预览**: <https://zhinianqaq.github.io/Valorant-Kill-Icon-Preview/>

---

## Features / 功能特性

| 中文 | English |
| --- | --- |
| 23 款皮肤，10 个系列下拉选择，带变种按钮（基础 / V1 / V2 / V3） | 23 skins, 10 series in the dropdown, variant buttons (Base / V1 / V2 / V3) |
| 击杀类型：普通 / 爆头（爆头红色填充 + 缩放期间红/原色闪烁，结束后保持红色；徽章红色闪光） | Kill type: Normal / Headshot (red-filled marker, red↔original blinking during the scale-in, stays red after; red emblem flash) |
| 连杀数 1–6（决定击杀数数量与旋转） | Combo 1–6 (sets kill-count & rotation) |
| 播放 / 重置 / 暂停、循环播放开关 | Play / Reset / Pause, loop toggle |
| 音效开关；播放动画时同步播放音效；5 杀以上无 6 杀音效时回退 5 杀音效；爆头与普通连杀同音效 | Sound toggle; sound plays with the animation; combos above 5 fall back to kill 5 sound when no kill 6 exists; headshot reuses the normal combo sound |
| 播放速度、缩放滑块（缩放滑块可实时缩放画面） | Speed and zoom sliders (zoom scales the preview live) |
| 9 个图层独立开关（粒子底爆 / 光环 / 击杀数 / 外框 / 刀刃 / 徽章 / 爆头标记 / 叠加粒子 / 图标闪光） | 9 independent layer toggles (base particle / halo ring / kill-count ring / frame / blade / emblem / headshot marker / overlay particles / icon flash) |
| 入场：徽章 → 约 3 帧后外框与击杀数淡入 → 再过 3 帧光环从上往下显现；出场：外框先消失，徽章同步缩小淡出，徽章消失后光环与击杀数变暗淡出 | Entry: emblem first → frame & kill-count fade in ~3 frames later → halo wipes in from top ~3 frames after; exit: frame disappears first while emblem shrinks & fades, then halo & kill-count fade out |
| 光环使用各系列 ring 贴图（无系列贴图时回退 `default_ring.png`），从上往下显现，单次播放不循环 | Halo uses each series' ring texture (falls back to `default_ring.png`), wipes in from top, plays once without looping |
| 粒子效果独立整体缩放（`--particle-global-scale`），不受 `--global-scale` 影响 | Particles have their own global scale (`--particle-global-scale`) independent of `--global-scale` |
| 每个皮肤系列可单独微调各图层尺寸（`size-tuning.css` 内 `body.skin-xxx` 区块） | Per-series layer-size tuning via `body.skin-xxx` blocks in `size-tuning.css` |
| 中 / 英界面切换（右上角按钮，默认中文） | Chinese / English UI toggle (top-right button, Chinese by default) |
| 皮肤参数摘要表（各图层贴图帧数与帧间隔） | Skin animation-param summary table (frames & interval per layer) |

---

## Skins / 皮肤列表

| English name | 中文名 |
| --- | --- |
| Valorant Default | 原皮 |
| Valorant Default MA | 原皮_v1 |
| Valorant Prime | 紫阙金琅2.0 |
| Valorant Glitchpop | 全息波普 |
| Valorant Singularity (+ V1/V2/V3) | 奇点（+ V1/V2/V3） |
| Valorant Gaia's Vengeance (+ V1/V2/V3) | 盖亚的复仇（+ V1/V2/V3） |
| Valorant Bubblegum Deathwish (+ V1/V2/V3) | 末日泡泡（+ V1/V2/V3） |
| Champions 2021 | 2021全球冠军赛 |
| Valorant Sovereign (+ V1/V2/V3) | 天界神兵（+ V1/V2/V3） |
| Valorant Elderflame | 上古龙炎 |
| Valorant Oni | 般若假面 |

---

## Usage / 使用说明

1. Download or clone this repository.
   下载或克隆本仓库。
2. Open `index.html` in any modern browser (Chrome / Edge / Firefox).
   用任意现代浏览器打开 `index.html`（Chrome / Edge / Firefox）。
3. No build step, no server required — `file://` works directly.
   无需构建、无需本地服务器，直接以 `file://` 打开即可。

## Project structure / 项目结构

```
Valorant-Kill-Icon-Preview/
├── index.html              # Main preview tool (inline CSS/JS) / 主预览工具（内联样式与脚本）
├── size-tuning.css         # Per-series layer-size tuning / 各系列图层尺寸微调
├── skin-name-mapping.md    # English–Chinese skin name mapping / 皮肤名称中英对照
├── assets/
│   ├── skin-data.js        # Skin data (23 entries, used at runtime) / 皮肤数据（23 条，运行时使用）
│   ├── skin-data.json      # Same data in JSON (source of truth) / 同数据 JSON（数据源）
│   ├── presets/            # Per-skin preset JSON files / 各皮肤预设
│   ├── textures/           # Sprite-sheet / texture images / 精灵图与贴图
│   └── sounds/             # Kill banner audio / 击杀图标音效
└── .gitignore
```

## Tuning layers with CSS / 用 CSS 微调图层

Open `size-tuning.css`. Global variables live under `:root`; per-series overrides live in `body.skin-<series>` blocks, e.g.:

打开 `size-tuning.css`。全局变量位于 `:root`，各系列覆盖位于 `body.skin-<series>` 区块，例如：

```css
:root {
  --global-scale: 1.0;           /* overall scale / 全部图层整体缩放 */
  --particle-global-scale: 1.0;  /* particle-only scale, independent of the above / 粒子独立缩放 */
  --layer-emblem-scale: 1.0;     /* emblem / 徽章 */
  --layer-frame-scale: 1.0;      /* frame / 外框 */
  --layer-bar-scale: 1.0;        /* kill-count ring / 击杀数 */
  --layer-halo-scale: 1.0;       /* halo ring / 光环 */
  --layer-headshot-scale: 1.0;   /* headshot marker / 爆头标记 */
  --halo-wipe-duration: 200;     /* halo wipe-in duration in ms / 光环显现时长（毫秒） */
}
body.skin-gaia {
  --layer-emblem-scale: 1.4;     /* Gaia example / 盖亚示例 */
}
```

## GitHub Pages / 在线部署

The repository is published at:

本仓库已发布在：

<https://zhinianqaq.github.io/Valorant-Kill-Icon-Preview/>

The site is a static page — push the files to the `main` branch and GitHub Pages serves `index.html` directly. All assets are referenced with relative paths, so no additional configuration is needed.

站点为静态页：将文件推送到 `main` 分支，GitHub Pages 直接托管 `index.html`。素材全部使用相对路径，无需额外配置。

---

## Disclaimer / 声明

This tool is an independent fan-made preview utility. Valorant and all related skin artwork are property of Riot Games. All textures and sounds are the property of their respective owners and are used here for preview purposes only.

本工具为独立制作的粉丝预览工具。Valorant 及其相关皮肤素材版权归 Riot Games 所有；所有贴图与音效归各自版权方所有，此处仅用于预览目的。
