k -n elasticsearch exec -it deployment/elasticsearch -- bin/elasticsearch-reset-password -u elastic
k -n elasticsearch exec -it deployment/elasticsearch -- bin/elasticsearch-create-enrollment-token --scope kibana
k -n elasticsearch logs deployment/kibana

k -n elasticsearch port-forward service/elasticsearch 9200

alias curl_wrapper='curl -k -vvv --user "elastic:$ELASTIC_PASSWORD" -H "Content-type: application/json"'

curl_wrapper -X POST https://localhost:9200/_security/role/filebeat -d @role-filebeat.json
curl_wrapper -X POST https://localhost:9200/_security/user/filebeat -d @user-filebeat.json
curl_wrapper -X PUT https://localhost:9200/_ilm/policy/filebeat-namespaces -d @lifecycle-filebeat-kubernetes.json
curl_wrapper -X PUT https://localhost:9200/_index_template/filebeat-kubernetes
-d @template-filebeat-kubernetes.json
