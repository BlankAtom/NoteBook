1、安装OpenSSH服务器

2、`Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'`

3、`Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`


4、`Start-Service sshd`

5、`Set-Service -Name sshd -StartupType 'Automatic'`

6、`Get-NetFirewallRule -Name *ssh*`
如果是放开的，那么结果会提示 `OpenSSH-Server-In-TCP`这个状态是 enabled。