# SMM

NiFi -> invokeHTTP to SMM REST API for different conditions

echo ""
echo ""
echo "▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔"
echo "Build Kafka Alerts"
echo "▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔"
echo ""
echo ""

curl -X GET "http://edge2ai-1.dim.local:9991/api/v1/admin/alert/notifications?limit=11&startOffset=-1"

# POST to build A Notifier

curl -X POST "http://edge2ai-1.dim.local:9991/api/v1/admin/notifiers" -H "accept: application/json" -H "Content-Type: application/json" -d '{"id":null,"name":"http","description":"http","notifierProviderId":"http","rateLimiterConfig":{"count":1,"duration":"MINUTE"},"config":{"URL":"http://edge2ai-1.dim.local:9999/alerts","ConnectionTimeoutInMilliSecs":30000,"ReadTimeoutInMilliSecs":30000}}'

echo ""

# GET to list that notifier
curl -X GET "http://edge2ai-1.dim.local:9991/api/v1/admin/notifiers"

echo ""

# POST to Add a new alert Policy
for f in /opt/demo/ApacheConAtHome2020/alerts/*.json
do 
  echo "Load alert $f"
  curl -X POST "http://edge2ai-1.dim.local:8585/api/v1/admin/alertPolicy" -H "accept: application/json" -H "Content-Type: application/json" -d @$f
  echo ""
done


# GET the list of alert policies created
curl -X GET "http://edge2ai-1.dim.local:9991/api/v1/admin/alertPolicy"

# To Disable all policies, iterate all policies, go by id
# http://edge2ai-1.dim.local:9991/api/v1/admin/alertPolicy/45/disable

# To DELETE all policies, iterate all policies, go by id
# http://edge2ai-1.dim.local:9991/api/v1/admin/alertPolicy/6

https://github.com/tspannhw/ApacheConAtHome2020/tree/main/alerts
