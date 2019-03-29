---
layout: post
title: 文件（Excel)下载
tags: Web_Dev
---

后台根据用户提交的数据动态生成文件，前端实现自动下载。

## 后台代码
```PHP
        $list = $this->abRepository->getOrderLogs();
        
        $spreadsheet = new Spreadsheet();
        $spreadsheet->setActiveSheetIndex(0)->setCellValue('A1', '会员ID')->setCellValue('B1', '订单号')->setCellValue('C1', '日志类型')->setCellValue('D1', '收入/支出')->setCellValue('E1', '账户')->setCellValue('F1', '金额');
        
        $i = 2;
        foreach($list['data'] as $record){
            $spreadsheet->setActiveSheetIndex(0)->setCellValue('A'.$i, $record['member_id'])->setCellValue('B'.$i, $record['order_no'])->setCellValue('C'.$i, $record['type_text'])->setCellValue('D'.$i, $record['way_text'])->setCellValue('E'.$i, $record['account_type_text'])->setCellValue('F'.$i, $record['money']);
            $i++;
        }
          
        $spreadsheet->getActiveSheet()->setTitle('OrderLogs');
        // Redirect output to a client’s web browser (Xlsx)
        header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
        header('Content-Disposition: attachment;filename="system_category.xlsx"');
        header('Cache-Control: max-age=0');
        // If you're serving to IE 9, then the following may be needed
        header('Cache-Control: max-age=1');
        $writer = IOFactory::createWriter($spreadsheet, 'Xlsx');
        $writer->save('php://output');
 ```
 
 ## 前端代码
 ```JavaScript
 	var downloadExcel = function (url, data) {
	    let xhr = new XMLHttpRequest();
	    //set the request type to post and the destination url to url
	    xhr.open('POST', url);
	    //set the reponse type to blob since that's what we're expecting back
	    xhr.responseType = 'blob';
	    xhr.setRequestHeader("Content-Type", "application/json");
	    xhr.send(JSON.stringify(data));

	    xhr.onload = function(e) {
	        if (this.status == 200) {
	            // Create a new Blob object using the response data of the onload object
	            var blob = new Blob([this.response], {type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'});
	            //Create a link element, hide it, direct it towards the blob, and then 'click' it programatically
	            let a = document.createElement("a");
	            a.style = "display: none";
	            document.body.appendChild(a);
	            //Create a DOMString representing the blob and point the link element towards it
	            let url = window.URL.createObjectURL(blob);
	            var rightNow = new Date();
	            var fName = rightNow.toISOString().slice(0,19) + 'order_log.xlsx';
	            a.href = url;
	            a.download = fName;
	            //programatically click the link to trigger the download
	            a.click();
	            //release the reference to the file by revoking the Object URL
	            window.URL.revokeObjectURL(url);
	        }else{
	            //deal with your error state here
	        }
	    };
	}
  
// Call the download function
downloadExcel("{:url('abc.com/processDonwload.php',['get_json'=> 'money_log'])}?id="+member_id, qData);
```

    
