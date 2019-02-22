# acefile

poc file of [extracting-code-execution-from-winrar](https://research.checkpoint.com/extracting-code-execution-from-winrar/)

check the file use command:
```
python acefile.py --headers 1.rar
```

When the 1.rar is unzipped, a file named demo2.txt will be released to dir `c:\c2\demo123\`.

漏洞利用：使用WinACE（只能使用这个软件）创建包含恶意文件的ace格式压缩文档（创建压缩文档时需要选择包含完整路径，同时为了满足payload长度需求最好确保完整文件路径长度足够--路径不包含磁盘分区号）,
修改filename值为特意制作的payload (Eg:C:\C:C:../AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\some_file.exe), 
根据数据的变化修改hdr_crc值确保校验通过（以上值均可通过上面的命令查看）

思考：例子payload可以保证当压缩文档在用户目录下的第一级目录（如Desktop，Dowloads）运行时释放恶意文件到自启动目录且不受用户名限制，但是如果在其他位置运行则会失败；而1.rar则直接硬编码将文件释放到一个固定目录，
那么，如果不考虑用户名限制，将释放固定目录的技术结合其他技术实现自启动是否可行？（lpk.dll; 注册表根据路径寻找系统程序并启动，释放恶意文件到高优先级目录实现劫持; 直接覆盖--有覆盖提示，很难用; 寻找特定的，有寻找加载文件执行行为的软件（多为加载dll等模块，但模块不是自带，可能不在，可以劫持），进行劫持; 其他。。。）
