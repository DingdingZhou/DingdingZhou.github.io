
## yii2框架中,导出CSV格式文件

```
function exportCSV($fileName,$query,$header){
	header('Content-Type: application/vnd.ms-excel'); 
	header('Content-Disposition: attachment;filename="user.csv"'); 
	header('Cache-Control: max-age=0'); 

	// 打开PHP文件句柄，php://output 表示直接输出到浏览器 
	$fp = fopen('php://output', 'a');
	// 将数据通过fputcsv写到文件句柄 
	fputcsv($fp, $head); 
	foreach($query->batch(1000) as $models){
		$arr=$models->toArray();
		foreach($arr as $row){
			fputcsv($fp, $row); 	
		}
		ob_flush(); 
		flush(); 
	}
}
```