fs模块

<h5>一.文件夹操作</h5>

1.fs.mkdirSync("./mydir");//创建文件夹
2.fs.rmdirSync("./mydir");//删除文件夹
3.var files=fs.readdirSync("./mydir");//获取目录下所有的文件，以数组形式存储



<h5>二.文件操作</h5>

1.写入文件

1.1异步:fs.writeFile(filename,data,[options],callback)

参数option说明：
*	encoding(编码默认utf8)
*	mode(文件权限默认:0666)
*	flag(打开行为:具体如下表默认为w)


|Flag|描述|
|--|--|
|r|读取模式|
|r+|读取模式|
|w|写入模式，文件不存在则创建|
|wx|读取模式，文件不存在则失败|
|w+|读写模式，文件不存在则创建|
|wx+|读写模式，文件不存在则失败|
|a|追加模式，文件不存在则创建|
|ax|读写模式，文件不存在则失败|
|a+|读取追加模式，文件不存在则创建|
|ax+|读取追加模式，文件不存在则失败|

```
fs.writeFile("./mydir/1.txt","abc",(err)=>{
if(err){throw err;}
console.log("写入成功");
});//创建文件并写入数据
```

```
//追加写入,因为默认flag是w,每次写入都会清除文本内容。改用a
fs.writeFile("./mydir/1.txt","def",{"flag":"a"},(err)=>{
if(err){throw err;}
console.log("写入成功");
});//创建文件并追加写入数据
```
1.2同步:fs.writeSync(filename,data,[options]);

2.读取文件

2.1异步 fs.readFile(path,[options],callback)

参数option说明：
*	encoding(编码默认utf8)
*	flag(打开行为:具体如下表默认为w)

注意：回调函数中的data是buffer数据，需要转化

```
fs.readFile("./mydir/3.txt","utf8",(err,data)=>{
    if(err){throw err;}
    console.log(data);
});
```

2.2同步 fs.readFileSync(path)

`console.log(fs.readFileSync("./mydir/3.txt").toString());`

3.删除文件
3.1异步 fs.unlink(path,callback)

```
fs.unlink("./mydir/1.txt",(err)=>{
     if(err){throw err;}
     console.log("删除成功");
     });//删除文件
```

3.2同步 fs.unlinkSync(path)

`fs.unlinkSync("./mydir/1.txt");`

4.文件是否存在

4.1异步 fs.exists(path,callback)//已废弃
4.2同步 fs.existsSync(path)

5.文件拷贝

5.1异步 fs.copyFile(src,dest,flags,callback)

参数说明：
*	src文件名
*	dest目标文件名
*	flags拷贝操作修饰符

5.2同步 fs.copeFileSync(src,dest)

6.追加数据

6.1异步 fs.appendFile(path,data,callback)

`fs.appendFile("./mydir/3.txt","\ndef");//追加`

6.2同步 fs.appendFileSync(path,data)

`fs.appendFile('./mydir/3.txt',"\ndef");//追加`

7.修改文件名

7.1异步 fs.rename(oldPath,newPath,callback)

```
fs.rename("./mydir/1.txt","./mydir/3.txt",(err)=>{
if(err){throw err;}
console.log("修改成功");
})
```
7.2同步 fs.rename(oldPath,newPath)

`fs.rename("./mydir/1.txt","./mydir/3.txt")`

8.获取文件信息

8.1异步 fs.stat(path,callback) //stats对象有很多属性，具体参考nodejs中文文档
```
fs.stat("./mydir/3.txt",(err,stats)=>{
    if(err){throw err;}
    console.log(stats);
});//获取文件信息
```
stats.isDirectory()//判断是否是文件夹
stats.isFile()//判断是否是文件


8.2同步 fs.stat(path)

9.监听文件
9.1 fs.watchFile(filename,[options],listener)

参数说明：
*	option
	*	persistent 默认值true//进程是否应该继续进行
    *	interval 默认值5007//频率
*	listener
	*	current//stat对象
	*	previous//stat对象

9.2 fs.watch(filename,[options],listener)

参数说明：
*	option
	*	persistent 默认值true//进程是否应该继续进行
    *	interval 默认值5007//频率
*	listener
	*	eventType//事件类型 主要是change和rename
	*	filename//修改的文件名

```
fs.watch("./mydir/1.txt",(eventType,filename)=>{
    console.log(`${eventType}`);//change
    console.log(`${filename}`);//1.txt
});
```