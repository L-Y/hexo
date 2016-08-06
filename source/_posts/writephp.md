title: php 输出类
date: 2015-03-21 17:00:44
categories: code
tags: [php]
---

php 输出类 代码片段

```bash
class output{
	public function alertmsg($msg){
		echo "<script type='text/javascript'>";
		echo "alert(\"$msg\");";
		echo "<\/script>";
	}
	public function locationmsg($msg,$goto){
		echo "<script type='text/javascript'>";
		echo "alert(\"$msg\")";
		if($goto){
			echo "location=\"$goto\";";
		}else{
			echo "history.go(-1);";
		}
		echo "<\/script>";	
	}
	public function httpequivmsg($im,$goto){
		echo "<meta http-equiv=\'Refresh\' content=".$im.";URL=".$goto.">";
	}
}


```
