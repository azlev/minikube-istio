k -n grafana create configmap grafana --from-file=grafana.ini=grafana.ini
curl -X "POST" "http://localhost:3000/api/datasources"     -H "Content-Type: application/json"      --user admin:admin      --data-binary @datasources.json
