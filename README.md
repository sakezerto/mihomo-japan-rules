# mihomo-japan-rules

面向 **Mihomo / Clash Meta** 的日本地区服务规则集。

本项目把两类规则合并为一个可直接给 Mihomo Rule Provider 使用的域名列表：

1. **SukkaW 日本区流媒体规则**：每天自动同步，用于 DMM/FANZA、Abema、niconico、Hulu Japan、TVer、NHK Plus 等日本区服务。
2. **手工维护的日本内容/日区服务规则**：用于 LINEマンガ、BookWalker、日本漫画站及部分日区游戏服务。

> 本项目不把所有 `.jp` 域名都代理到日本，而是只收录具有明显日本地区属性、或经常需要日本出口的服务。

## Rule Provider

直接在 Mihomo 中使用：

```yaml
rule-providers:
  japan-only:
    type: http
    behavior: domain
    format: text
    url: "https://raw.githubusercontent.com/sakezerto/mihomo-japan-rules/main/rules/japan-only.list"
    path: ./ruleset/japan-only.list
    interval: 86400

rules:
  # 放在中国大陆规则之前、最终 MATCH 之前
  - RULE-SET,japan-only,🇯🇵 日本
```

## 自动更新

GitHub Actions 每天运行一次，并支持手动执行：

```text
SukkaW stream_jp.txt
        ↓
转换 DOMAIN / DOMAIN-SUFFIX
        ↓
合并 manual/japan-content.list
        ↓
去重、排序
        ↓
rules/japan-only.list
```

只有生成结果发生变化时才会提交新版本。

## 文件

- `rules/japan-only.list`：Mihomo 直接使用的最终规则。
- `manual/japan-content.list`：手工维护的日本内容/日区服务。
- `.github/workflows/update.yml`：每日自动构建。

## 上游

Sukka Ruleset Server：
https://ruleset.skk.moe/

日本区流媒体规则：
https://ruleset.skk.moe/Clash/non_ip/stream_jp.txt

本项目仅对上游规则做格式转换和增补，不替代上游项目。
