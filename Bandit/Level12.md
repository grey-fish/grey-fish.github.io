# Bandit

## Level 12
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

<br/>
## Solution
In order to get the original file back from hexdump, we use `xxd` command

```shell
$ cat data.txt | xxd -r > data
```

Screenshot:

![Level 12 Image](./images/Level12.png)

The resulting file: `data` is a gzip compressed file. Below we decompress it multiple times using different utilities accordingly.

Below is the session log

```shell
bandit12@bandit:/tmp/test2$ cat data.txt | xxd -r > data     
bandit12@bandit:/tmp/test2$ file data            
data: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix  
bandit12@bandit:/tmp/test2$ mv data data.gz                            
bandit12@bandit:/tmp/test2$ gunzip data.gz                                                       
bandit12@bandit:/tmp/test2$ ls -lrth                                                             
total 8.0K                                          
-rw-r----- 1 bandit12 root 2.6K Aug  6 09:50 data.txt                                                   
-rw-r--r-- 1 bandit12 root  573 Aug  6 09:50 data  

bandit12@bandit:/tmp/test2$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/test2$ bunzip2 data
bunzip2: Can't guess original name for data -- using data.out
bandit12@bandit:/tmp/test2$ ls -lrth
total 8.0K
-rw-r----- 1 bandit12 root 2.6K Aug  6 09:50 data.txt
-rw-r--r-- 1 bandit12 root  431 Aug  6 09:50 data.out
bandit12@bandit:/tmp/test2$ file data.out
data.out: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
bandit12@bandit:/tmp/test2$ mv data.out data.gz
bandit12@bandit:/tmp/test2$ gunzip data.gz
bandit12@bandit:/tmp/test2$ ls -lrth
total 24K
-rw-r----- 1 bandit12 root 2.6K Aug  6 09:50 data.txt
-rw-r--r-- 1 bandit12 root  20K Aug  6 09:50 data
bandit12@bandit:/tmp/test2$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/test2$ tar -xvf data
data5.bin
bandit12@bandit:/tmp/test2$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/test2$ tar -xvf data5.bin
data6.bin
bandit12@bandit:/tmp/test2$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/test2$ bunzip2 data6.bin
bunzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/test2$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/test2$ tar -xvf data6.bin.out
data8.bin
bandit12@bandit:/tmp/test2$ file data8.bin 
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
bandit12@bandit:/tmp/test2$ mv data8.bin data8.gz
bandit12@bandit:/tmp/test2$ gunzip data8.gz
bandit12@bandit:/tmp/test2$ ls -lrth
total 52K
-rw-r--r-- 1 bandit12 root   49 May  7  2020 data8
-rw-r--r-- 1 bandit12 root  10K May  7  2020 data6.bin.out
-rw-r--r-- 1 bandit12 root  10K May  7  2020 data5.bin
-rw-r----- 1 bandit12 root 2.6K Aug  6 09:50 data.txt
-rw-r--r-- 1 bandit12 root  20K Aug  6 09:50 data
bandit12@bandit:/tmp/test2$ file data8
data8: ASCII text
bandit12@bandit:/tmp/test2$ cat data8
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```
