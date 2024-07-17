# 注意
- これは試験的なコードであり、 以下を含めてセキュリティ・コンプライアンス・コスト等考慮していない点が多くありサービスを一般公開すると問題あるコードです
- Node v16で動作します。Nodeのバージョンをそれ以上にあげるとエラーがでます（未解決）
- Nuxt2です。Nuxtのバージョンあげたい（未）
- Lambda側が認証何もなしなので、LambdaのURLを知ってさえいればなんでもできます

# 設定
-  https://github.com/nanananakam/ivs-simple でdeployした Lambda Function のURLを `LAMBDA_URL` として設定

# 概要
- `/start`で配信開始。rtmpのurlが取得できます
- 配信開始するとHomeに配信視聴へのリンクが表示されます



