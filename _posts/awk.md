#!/bin/bash

awk '{
	for (i=1;i<NF;i++){
		printf $i "\t";
	}
	cmd=("date +%Y-%m-%d-%H -d @" $NF);
	system(cmd);
	printf("\n"); 
}' $1;



