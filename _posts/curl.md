## curl 命令 POST 使用示例
```
#!/bin/bash

url='http://localhost:9876';

parms=(
'{"client_id":"180100031016","user_id":"3150000067","url":"http://localhost:8647","path":"/user/show"}'
'{"client_id":"180100031016","user_id":"3150000067","url":"http://localhost:8647","path":"/user/show"}'
'{"client_id":"180100031016","user_id":"3150000067","url":"http://localhost:8647","path":"/user/show"}'
)

for parm in ${parms[@]}
do

res=`(curl $url -s -d $parm -H "Content-Type: application/json")`
jq '.' <<< $res 

done
```









