# aimercat-self-use-rules
自用规则补充

## 文件

- `rules/My_Direct_Domain.yaml` — 直连域名规则集，**classical 格式**（`payload:` + 完整规则语法，支持 `DOMAIN-SUFFIX` / `DOMAIN-KEYWORD` 等）
- `rule_provider_snippet.yaml` — 在 mihomo 配置里引用本规则集的写法

## 用法

`rule-providers` 区域：

```yaml
rule-providers:
  my_direct:
    type: http
    interval: 86400
    behavior: classical
    format: yaml
    url: "https://raw.githubusercontent.com/aimercat1994/aimercat-self-use-rules/main/rules/My_Direct_Domain.yaml"
```

`rules` 区域的 `MATCH` 之前：

```yaml
  - RULE-SET,my_direct,DIRECT
```

国内直连可用 jsdelivr 镜像（更快）：

```
https://cdn.jsdelivr.net/gh/aimercat1994/aimercat-self-use-rules@main/rules/My_Direct_Domain.yaml
```

## 为什么用 yaml 而不是 mrs

`.mrs` 是编译后的二进制（zstd 压缩的后缀 trie），有两个问题：

1. **不可读、不可复现** —— 仓库里没有它的源文件，改一条域名必须用 mihomo 的 `convert-ruleset` 重新编译。
2. **语义受限** —— `behavior: domain` 的 MRS 只能做精确/后缀匹配，**无法表达 `DOMAIN-KEYWORD`**。此前 `banzhu` 被当成字面主机名烧进 trie（永久死条目），`DOMAIN-KEYWORD,banzhu` 实际从未生效。

classical + yaml 直接使用规则文本，上面两个问题都不存在。
