# MySQL Replication설정 방법에 대해서 작성하겠습니다.

Replication 설정 전 확인
- Master서버와 Slave서버의 server_id값은 모두 달라야 합니다. Replication설정전에 my.cnf에서 server_id값을 확인 및 수정합니다.
- log_bin의 값을 확인합니다. binary_log 파일명을 입력합니다.
- Replication전용 User를 작성합니다.
```
mysql> CREATE USER 'repl'@'%.example.com' IDENTIFIED BY 'password';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%.example.com';
```
  
Master 서버에서 실행
 - Replication설정에 필요한 정보를 확인합니다.
 - File값과 Position값을 확인합니다.
```
mysql> show master status¥G
```
Slave 서버에서 실행
 - Master 서버에서 확인한 Replication에 필요한 설정값을 확인합니다.

```
mysql> show slave status¥G    ///Replication설정이 되어 있지 않은 상태를 확인합니다.

mysql> CHANGE MASTER TO
  MASTER_HOST='Master 서버명',
  MASTER_USER='Replication 유저명',
  MASTER_PASSWORD='Replication 유저 비밀번호',
  MASTER_LOG_FILE='binary_log명',              ///Master 서버에서 show master status명령어로 확인한 File값을 입력합니다.
  MASTER_LOG_POS=포지션 번호;                    ///Master 서버에서 show master status명령어로 확인한 Position값을 입력합니다.

mysql> show slave status¥G    ///Change master to명령어로 입력한 값들을 확인합니다. Second_Behind_Master값은 NULL입니다.(Master_Host, Master_User, Master_Log_File, Read_Master_Log_Pos)
mysql> start slave;           ///Replication설정을 시작합니다.
mysql> show slave status¥G    ///Second_Behind_Master값에 NULL이 아닌 숫자가 들어있음을 확인합니다.
```
