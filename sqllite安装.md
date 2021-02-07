# 安装

## 下载

```bash
wget http://www.sqlite.org/sqlite-3.5.6.tar.gz
```

## 解压

```bash
tar -xzvf sqlite-3.5.6.tar.gz
```

## 安装

```bash
cd sqlite-3.5.6
./configure --disable-tcl ##加上这个选项则不需要TCL,否则在2.4内核上编译通不过
make
make install
```

## 测试

```bash
./sqlite3 text.db 
```

如果成功，出现

```bash
SQLite version 3.5.6
Enter ".help" for instructions
sqlite>
```

# golang操作
