# podsync-helm-chart

An unofficial helm chart for podsync.

# Example usage:

```sh
helm repo add my0npodsync https://my0n.github.io/podsync-helm-chart
helm repo update
helm install my0npodsync/podsync -f values.yaml
```

```yaml
# values.yaml
configuration:
  reloaderEnabled: true
  template: |
    [server]
    port = 80
    hostname = "http://${PODSYNC_INGRESS_HOST}"
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
  dataClaimName: podsync-pvc
ingress:
  enabled: true
  host: podsync.example.local
```

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

```yaml
# podsync-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: podsync-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
  storageClassName: my-storage-class
  volumeMode: Filesystem
```
