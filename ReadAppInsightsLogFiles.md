# Viewing Logs in Application Insights

## View log entries in Application Insights

```sql
traces 
| project timestamp, customDimensions["eventSource"], message, customDimensions.LogLevel, severityLevel, customDimensions
// | where customDimensions.eventSource  startswith "Interaction"
// | where customDimensions.eventSource  startswith "Fanout"
// | where customDimensions.eventSource  startswith "Sequential"
| filter customDimensions["eventSource"] != '' 
and timestamp >= ago(30m) // ago(1d) ago(8h) ago(15m) 
| extend source = tostring(customDimensions["eventSource"])
| take 200
| order by timestamp desc 
```

---

## View every time this function was called, not just the trace info

```sql
requests
| project
    timestamp,id,operation_Name,success,resultCode,duration,operation_Id,cloud_RoleName,invocationId=customDimensions['InvocationId']
| where timestamp > ago(1d)
| where cloud_RoleName =~ 'lll-durabledemo-dev' and operation_Name =~ 'Interaction_Orchestration'
| order by timestamp desc
| take 200
```
