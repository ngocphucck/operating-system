1. Bash shell binary programme:  ```$ /usr/bin/bash```
2. Path to current directory: ```$ pwd```
3. Home directory: Storage directory for user
```$ cd ~```
4. Root directory: Top directory of the hierarchy directory tree
```$ cd /```
5. Files & directory in current directory: ```$ ls```
6. Find path: ```$ which file_name```
7. Create a new directory: ```$ mkdir directory_name```
8. Check the exist of a file (if not exist -> create an empty file): ```$ touch file_name```
9. Write something to stdout file: ```$ echo sth```
10. Output redirection (Transfer content from stdout to a specific file): ```$ echo "sth" > file_name```
11. Append content from stdout to a specific file: ```$ echo "sth" >> file_name```
12. Concatenate the content of files and write to stdout file: ```$ cat file1 file2 ...```
13. Copy a directory: ```$ cp -r org_dir tar_dir```
14. Untar with gzip: ```$ tar -xzvf file.tar```
15. Check whether there is a keyword in a file: ```$ grep sth file_name```
16. Pipeline operator, output of the previous command is writen to stdout and the content in stdout is input of the next command: ```$ command1 | command2 | command3 ....```
17. Check whether a keyword is in the file and print out a line containing this keyword: ```$ grep file_name | wc```
18. Write content of k lines in a file to stdout: ```$ head -n k file_name```
19. Write content of k last lines in a file to stdout: ```$ tail -n k file_name```
20. Write content of the column k in a file to stdout: ```$ cat file_name | awk '{print $k}'```
21. Remove a line containing a keyword in stdout: ```$ cat file_name | grep -v keyword```
22. Sort content in stdout: 
```$ cat file_name | sort ```
(reverse) ```$ cat file_name | sort -r```
(numeric comparison) ```$ cat file_name | sort -n```
(comparison in column x) ```$ cat file_name | sort -k x```
23. Add parameters to a command: ```$ xargs param_name param_value command```
