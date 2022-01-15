# podsync-helm-chart

An unofficial helm chart for [podsync](https://github.com/mxpv/podsync) (currently using [this version](https://github.com/tuxpeople/docker-podsync) for yt-dlp support). I made this for my own learning purposes, but maybe you'll get some use out of it, too!

# Example usage:

Install the helm chart:

```sh
helm repo add my0npodsync https://my0n.github.io/podsync-helm-chart
helm repo update
helm install podsync my0npodsync/podsync -f values.yaml
```

Sample values.yaml given below; see [values.yaml](charts/podsync/values.yaml) for more details.

```yaml
# values.yaml
configuration:
  reloaderEnabled: true
  template: |
    [server]
    hostname = "http://${PODSYNC_HOST}"
    port = ${PODSYNC_PORT}
    data_dir = "/app/data/"

    [tokens]
    youtube = "${PODSYNC_YOUTUBE_KEY}"

    [feeds]
      [feeds.ID1]
      url = "https://www.youtube.com/channel/UCxC5Ls6DwqV0e-CYcAKkExQ"
  env:
    - name: PODSYNC_YOUTUBE_KEY
      valueFrom:
        secretKeyRef:
          name: youtube-api-key
          key: apiKey
persistence:
  enabled: true
  size: 100Gi
ingress:
  enabled: true
  host: podsync.example.local
```

Example kubernetes secret:

```yaml
# youtube-api-key.yaml
apiVersion: v1
kind: Secret
metadata:
  name: youtube-api-key
  namespace: default
type: Opaque
data:
  apiKey: WU9VUl9ZT1VUVUJFX0FQSV9LRVlfSEVSRQ==
```
