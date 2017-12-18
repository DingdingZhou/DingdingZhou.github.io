

// 逐行取出数据，不浪费内存 
while ($row = $stmt->fetch(Zend_Db::FETCH_NUM)) { 
	$cnt ++; 
	if ($limit == $cnt) { //刷新一下输出buffer，防止由于数据过多造成问题 
	ob_flush(); 
	flush(); 
	$cnt = 0; 
	} 

	foreach ($row as $i => $v) { 
	$row[$i] = iconv('utf-8', 'gbk', $v); 
	} 
	fputcsv($fp, $row); 
}






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





















