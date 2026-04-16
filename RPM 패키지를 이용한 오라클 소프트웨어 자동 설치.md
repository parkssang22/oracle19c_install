## vm을 설치한 후 모바텀에 연결한다
<img width="1311" height="611" alt="모바텀연결" src="https://github.com/user-attachments/assets/8ee4cc5f-2911-4cd1-8b61-ff217692eb84" />

## 모바텀에 다운받은 rpm 파일을 올린다
<img width="927" height="620" alt="rpm 모바텀에 올리기" src="https://github.com/user-attachments/assets/b8760e96-a249-4124-8dab-0769aea263b4" />

```bash
[root@localhost ~]# mv /home/oracle/oracle-database-ee-19c-1.0-1.x86_64.rpm .
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  oracle-database-ee-19c-1.0-1.x86_64.rpm
[root@localhost ~]# yum -y localinstall oracle-database-ee-19c-1.0-1.x86_64.rpm
```
## root 계정에서 yum 명령어를 이용해 rpm 파일을 설치한다
<img width="880" height="865" alt="rpm 성공" src="https://github.com/user-attachments/assets/0a0d3aee-b66b-4e89-a930-ad8ca393caed" />

## 설치하면 11g에서 수동으로 했던 파라미터 및 유저 리소스 설정, 유저 생성, 권한이 설정된 것을 확인할 수 있고 오라클 소프트웨어까지 자동으로 설치된 것도 볼 수 있다
```bash
[root@localhost ~]# id oracle
uid=1000(oracle) gid=1000(oracle) groups=1000(oracle),54321(oinstall),54322(dba),54323(oper),54324(backupdba),54325(dgdba),54326(kmdba),54330(racdba)
[root@localhost ~]# ls -dl /opt/oracle/product/19c/dbhome_1
drwxr-xr-x. 69 oracle oinstall 4096  4월 16 14:50 /opt/oracle/product/19c/dbhome_1
[root@localhost ~]# cd /opt/oracle/product/19c/dbhome_1
[root@localhost dbhome_1]# ls
OPatch       crs        diagnostics    inventory  network      oss       root.sh        srvm
QOpatch      css        dmu            javavm     nls          oui       runInstaller   suptools
R            ctx        drdaas         jdbc       odbc         owm       schagent.conf  ucp
addnode      cv         dv             jdk        olap         perl      sdk            usm
apex         data       env.ora        jlib       opmn         plsql     slax           utl
assistants   dbjava     has            ldap       oraInst.loc  precomp   sqldeveloper   wwg
bin          dbs        hs             lib        oracore      racg      sqlj           xdk
cfgtoollogs  deinstall  install        md         ord          rdbms     sqlpatch
clone        demo       instantclient  mgw        ords         relnotes  sqlplus
```

## SELINUX=disabled로 변경하고 오라클 패스워드 설정 및 오라클 환경 변수를 등록한다
```bash 
[root@localhost ~]# vi /etc/selinux/config
[root@localhost ~]# cat /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted

[root@localhost ~]# passwd oracle
oracle 사용자의 비밀 번호 변경 중
새  암호:
잘못된 암호: 암호는 8 개의 문자 보다 짧습니다
새  암호 재입력:
passwd: 모든 인증 토큰이 성공적으로 업데이트 되었습니다.
[root@localhost ~]# su - oracle
마지막 로그인: 목  4월 16 14:50:17 KST 2026
[oracle@localhost ~]$ vi .bash_profile
[oracle@localhost ~]$ cat .bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/.local/bin:$HOME/bin

export PATH
export ORACLE_BASE=/opt/oracle
export ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
export ORACLE_SID=ORCLCDB
export PATH=$PATH:$ORACLE_HOME/bin
```
