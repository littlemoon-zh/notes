
## tar

[reference](https://linuxize.com/post/how-to-create-and-extract-archives-using-the-tar-command-in-linux/)

`-c` --create 创建archive文件

`-z` compress the tar, default with gzip

`-t` list files and dir in tar or compressed tar

`-x` --extract 提取archive文件，可以提取被gzip压缩后的文件，end with `tar.gz`

`-f` to file or from file, otherwise to stdout


Example:

```shell
tar -tf x.tar.gz. # list file in tar

tar -xf xx.tar.gz # extract tar, should add -f, otherwise would output to stdout

tar -cf new.tar.gz file1 file2 file...  # create tar using file1 2...

tar -cf new.tar mydir # create tar using mydir

```
