# 医学英语斩词 · med-vocab-cards

学术英语（医学）Unit 1–8 课程词表的手机背词网页。单文件、无框架、无后端，可离线，能加到主屏幕当 App 用。

在线地址：**https://zyf2492313716-cloud.github.io/med-vocab-cards/**

## 四种练法

| 模式 | 玩法 |
|---|---|
| 背词 | 先看英文 + 音标回忆释义 → 显示释义 → 点「😎 认识 / 🤔 模糊」，答案里带助记法 |
| 选义 | 看英文，四选一挑中文释义 |
| 拼写 | 看中文拼英文，自动判分 |
| 填空 | 课程原句挖空，把要填的词敲进去 |

配图按释义自动配 emoji，🔊 朗读走浏览器语音（离线可用）。答对弹「+1 斩」，顶栏显示今日 x/20。

每张卡还有两样东西：

- **音标**：Oxford 英式/美式双音标一键切换（卡片上点 `🇬🇧 英` / `🇺🇸 美` 胶囊直接切换并切换发音语音），填空词蓝色加粗
- **助记法**：词根词缀（🔤）、构词拆分（✂️）、联想（💡）、搭配（📎），248 条全有

设置里可以单独关掉音标或助记法，也能调字号、深浅色、每日目标、随机顺序。

## 进度存在哪

进度（认识 / 模糊 / 错题 / 收藏 / 今日计数）只存在**这台设备的浏览器 localStorage** 里，
没有账号、不上传任何地方，也没有请求发往第三方。

换设备时：设置 → 进度备份 → 「导出进度」拿到一段文本码，在新设备「导入进度」粘回去即可。

## 本地预览

```bash
python3 -m http.server 8000
# 打开 http://127.0.0.1:8000
```

直接双击 `index.html` 用 file:// 打开也能用，只是 Service Worker 不生效（离线缓存需要 http）。

## 打开与安装

手机/电脑浏览器直接访问上面的在线地址。iPhone Safari 打开后点「分享 → 添加到主屏幕」，
就是一个全屏独立图标，断网也能打开（Service Worker 已缓存全部资源）。

重新部署：仓库设置 → Pages → Source 选 `Deploy from a branch` → 分支 `main` / 目录 `/ (root)`。

改了 `index.html` 之后，把 `sw.js` 里的 `VERSION` 往上加一位（`zanci-v2`），
否则手机端会继续用旧缓存。

## 数据

248 条词条来自教材《学术英语（医学）》Unit 1–8 各单元的 Expressions 词表页，
其中 208 条带课程原句挖空。字段结构：

```json
{ "id": 0, "u": 1, "t": "A doctor's life", "p": "neuron / neuronal overload",
  "i": "/ˈnjʊərɒn / ˈnjʊərəʊnl ˌəʊvəˈləʊd/", "ia": "/ˈnjʊərɒn / ˈnjʊərəʊnl/",
  "h": "✂️ neuron 神经元 + -al 形容词；overload = over 过度 + load 负荷",
  "c": "神经过载", "a": "neuron / neuronal", "s": "" }
```

`p` 英文、`i` 整词音标、`ia` 填空词的音标、`h` 助记法、`c` 中文释义、
`a` 课程里挖空要填的词、`s` 挖空句（`__BLANK__` 是空位标记）。

**音标来源：Oxford Learner's Dictionaries（LDOCE）英式标音**，逐词抓取后统一处理
（`vocab_web/build_ox_lexicon.py`）。420 个词里 369 个直接命中 Oxford 词条，
14 个由原形 + 词尾读音规则推出（复数/过去式/-ing），37 个（医学专名、连字符短语、
Oxford 未单独收词形的）走人工兜底表 `vocab_web/ipa_final_fix.json`，优先级最高。

之前用开源维基词典数据时，160 个词的标音与 Oxford 不一致（含 `vaccine` 这类明显错的），
已全部以 Oxford 为准替换；旧的维基版留在 `vocab_web/lexicon.wikt.bak.json` 备查。

发现别扭的音标，往 `vocab_web/ipa_final_fix.json` 加一条，重跑
`build_ox_lexicon.py` → `make_vocab_web.py` 即可。

数据仅供个人学习使用；词表本身来自教材与配套课程，重分发前请自行确认版权边界。

## 目录

```
index.html              应用本体（数据已内联）
manifest.webmanifest    PWA 清单
sw.js                   离线缓存（改版本号为 zanci-v2 等可强制刷新）
icons/                  应用图标
```
