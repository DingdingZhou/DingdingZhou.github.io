#!/bin/bash

parms=("ab" "cd");

for parm in ${parms[@]}
do

res=`(curl http://localhost:8081/test -s -d name=$parm)`
jq '.' <<< $res 

done






