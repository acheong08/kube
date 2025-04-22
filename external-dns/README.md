# External DNS with Cloudflare

`kubectl create secret generic cloudflare-api-key --from-literal=apiKey=YOUR_API_TOKEN`

`helm upgrade --install external-dns external-dns/external-dns --values values.yaml`
