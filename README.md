# 注意
- Node v16で動作します。Nodeのバージョンをそれ以上にあげるとエラーがでます（未解決）
- Nuxt2です。Nuxt3に上げたい（未）
- セキュリティ等考慮していない点が多くありサービスを一般公開すると問題あるコードです

# 設定
- 環境変数 `lambdaUrl` に https://github.com/nanananakam/ivs-simple でdeployした Lambda Function のURLを設定してください

# 概要
- `/start`で配信開始。rtmpのurlが取得できます
- 配信開始するとHomeに配信視聴へのリンクが表示されます



