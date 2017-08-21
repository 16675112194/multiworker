# MultiWorker PHP����̹�����

Multiworker�Ǵ�PHPʵ�ֵĶ���̹�������ʹ��master-worker����ģ�ͣ��������������µĶ���̵��ȡ����������������̱����Զ��ָ�����ʵ�����ơ�

��Ŀ��ҳ https://github.com/beyosoft/multiworker

bug��ʹ�÷�����zhangxugg@163.com

## һ���ص㣺
1.  ʹ��master-worker����ģ�ͣ��ȶ��ɿ��� Multiworker����ʵ����һ��master���̣���Ϊ�����̣������Worker���̣���Ϊ�������̻��ӽ��̣���ɣ���������Ҫ�������̵����ɡ����˳�״̬��⡣һ���й��������쳣�˳��������̾ͻ�����������һ���������̣������������������Ϊ�����̲��������ҵ���߼�������û���쳣�˳��Ŀ��ܡ�

2.  �����������ⲿ������ֻ��һ�����ļ��������������κ���Ŀ�Ϳ�ܡ�

3.  ֧�ֵ�ʵ�����ܣ����crontab��ʵ�ָ߿ɿ��Եĺ�̨���� ʵ��ҵ���У���������ϣ��һ�������ɵ�ʵ�����У�ֻ��ǰһ��ʵ���쳣�˳�ʱ���µ�ʵ�����ܳɹ����У����crontab��Multiworker�ĵ�ʵ�����ܣ����Ժ�����ʵ��һ���߿ɿ��ĺ�̨����

4.  ����������ȴ�����ͬ�Ĺ������̿ɸ���ͬ����������ȵ����̿ɼ����������������Ч�ʡ�

5. ��������״̬��⡣������������ָ��������״̬�˳��������̲����������µ��ӽ��̣������й���������ָ��������״̬�˳�ʱ����������Ϊ��������ϣ��Լ�ͬʱ�˳���

6.  �źſ��ƺͽ������п��ơ�ʵ���������У��������̷���SIGTERM�ź�ʱ�������̻���ÿ���ӽ��̷����źţ���֪�估ʱ�˳��������й��������˳���������Ҳ�˳���

## ��������Ҫ��
1.  ��Ϊʹ��Linux�źſ��ƣ���Ҫposix��չ��ֻ֧��Linux��ϵͳ����֧��windows��
2.  ��Ҫphp 5.3+��

## ����ע������
1. �������̿��Թ���һ�����ݿ�������Դ��
    
    ���Բ��ܣ�ÿ���������̱������½���һ�����ݿ����ӣ�����������������ϵĽ����������onWorkerStart�ص��йر��������Ѿ����������ݿ����ӣ������´򿪼��ɡ�
2. ���ʵ�ֲ�ͬ�Ĺ�������ִ�в�ͬ������
    
    onWorkerStart�ص��Ĳ��������ǽ��̵�PIN(process index number), ����0��ʼ��ţ�����ͨ���ж�PIN�Ӷ����ӽ�����ɲ�ͬ������

3. �ɿ�����Σ�

    �þ���������ʵ�ʳ������У������ʹ�á�

## �ġ�������
1.  ͨ�÷���
```php
$mp = new MulitWorker();
$mp->workerNum = 6;     //6����������
$mp->normalExitCode = 0; //����������0״̬���˳�ʱ �϶�Ϊ����״̬ ���������µĹ�������
$mp->debug = false;
# onWorkerStart�ص����ӽ�������
$mp->onWorkerStart = function($pin) use($mp) {
    //$pin��ָ�������̵ı��(process index number), ��0��ʼ��ͨ��$pin���ǿ��԰��Ź������̴���ͬ������
    $mp->Log("$pin started .\n");
    sleep(rand(5, 20));
    exit(41);   //ģ�⹤�������쳣�˳�  �����̻���������һ���ӽ��̽������Ĺ���
};

$mp->run();
```
����ʵ������syslog�м�¼��־��
```bash
[root@bogon ~]# tailf /var/log/messages

Aug 21 15:37:00 bogon multiworker[6689]: 5 started .
Aug 21 15:37:00 bogon multiworker[6687]: 3 started .
Aug 21 15:37:00 bogon multiworker[6685]: 1 started .
Aug 21 15:37:00 bogon multiworker[6690]: 0 started .
Aug 21 15:37:00 bogon multiworker[6688]: 4 started .
Aug 21 15:37:00 bogon multiworker[6686]: 2 started .
Aug 21 15:37:07 bogon multiworker[6684]: worker 6688(4) exit(41)
Aug 21 15:37:07 bogon multiworker[6684]: for worker for PIN: 4
Aug 21 15:37:07 bogon multiworker[6691]: 4 started .
Aug 21 15:37:08 bogon multiworker[6684]: worker 6689(5) exit(41)
Aug 21 15:37:08 bogon multiworker[6684]: for worker for PIN: 5
Aug 21 15:37:08 bogon multiworker[6692]: 5 started .
Aug 21 15:37:14 bogon multiworker[6684]: worker 6692(5) exit(41)
Aug 21 15:37:14 bogon multiworker[6684]: for worker for PIN: 5
Aug 21 15:37:14 bogon multiworker[6693]: 5 started .
Aug 21 15:37:16 bogon multiworker[6684]: worker 6687(3) exit(41)
Aug 21 15:37:16 bogon multiworker[6684]: for worker for PIN: 3
Aug 21 15:37:16 bogon multiworker[6694]: 3 started .
Aug 21 15:37:18 bogon multiworker[6684]: worker 6685(1) exit(41)
Aug 21 15:37:18 bogon multiworker[6684]: for worker for PIN: 1
Aug 21 15:37:18 bogon multiworker[6695]: 1 started .

```
�ɷ��֣������������쳣�˳�ʱ��multiworker�����������ӽ��̽����乤�����Ӷ�ʵ�ָ߿ɿ��ԡ�

2. �źŲ��񼰿��ơ�Multiworkerʹ���źŻ��ƣ�����ҵ��ѭ��ʱ���������źŵĲ��񣬴Ӷ����ܵ��¹������̲����źſ��ơ�������ҵ��ѭ���м�ʱ�����ź�ʹ֮�ܿء�
```php
$mp = new MulitWorker();
$mp->workerNum = 6;     //6����������
$mp->normalExitCode = 0; //����������0״̬���˳�ʱ �϶�Ϊ����״̬ ���������µĹ�������
$mp->debug = false;
$mp->onWorkerStart = function($pin) use($mp) {
    //$pin��ָ�������̵ı��(process index number), ��0��ʼ��ͨ��$pin���ǿ��԰��Ź������̴���ͬ������
    $mp->Log("$pin started .\n");
    for($i=0; $i <1000; $i++){
        $mp->signalDispatch(); //��ʱ�����ź�ʹ�ӽ����ܿ�
        // ҵ���߼�
    }
    exit(0); //ҵ������� �����˳�
};

$mp->run();
```

3. �źż����̿���ʹ�÷�����
```php
$mp = new MulitWorker();
$mp->workerNum = 6;
$mp->normalExitCode = 0;
$mp->debug = false;
$mp->onWorkerStart = function($pin) use($mp) {
    $mp->Log("$pin started .\n");
    sleep(3600);
    exit(0);
};

$mp->run();
```
�������ϴ��룬 �鿴���������ҳ������̲��ֹ����������˳��źţ�������ʵ���˳��������ÿ�����̷����˳��źš�

```bash
[root@bogon ~]# pstree -Ap
init(1)-+-auditd(1288)---{auditd}(1289)
        |-crond(1478)
        |-dhclient(1151)
        |-master(1464)-+-pickup(6132)
        |              `-qmgr(1477)
        |-mingetty(1526)
        |-mingetty(1528)
        |-mingetty(1530)
        |-mingetty(1532)
        |-mingetty(1534)
        |-mingetty(1536)
        |-nginx(1518)---nginx(1520)
        |-php(6684)-+-php(7086)
        |           |-php(7087)
        |           |-php(7088)
        |           |-php(7089)
        |           |-php(7090)
        |           `-php(7091)
        |-php-fpm(5787)-+-php-fpm(5788)
        |               `-php-fpm(5789)
        |-rsyslogd(1311)-+-{rsyslogd}(1312)
        |                |-{rsyslogd}(1313)
        |                `-{rsyslogd}(1314)
        |-sshd(1385)-+-sshd(1672)---bash(1676)
        |            |-sshd(1699)---bash(1703)---pstree(7092)
        |            `-sshd(5684)---bash(5688)
        `-udevd(516)-+-udevd(1543)
                     `-udevd(1544)
[root@bogon ~]# kill 6684  //�������̷����˳��ź�
[root@bogon ~]# pstree -Ap //�鿴��������ʵ���Ѿ�����ֹ
init(1)-+-auditd(1288)---{auditd}(1289)
        |-crond(1478)
        |-dhclient(1151)
        |-master(1464)-+-pickup(6132)
        |              `-qmgr(1477)
        |-mingetty(1526)
        |-mingetty(1528)
        |-mingetty(1530)
        |-mingetty(1532)
        |-mingetty(1534)
        |-mingetty(1536)
        |-nginx(1518)---nginx(1520)
        |-php-fpm(5787)-+-php-fpm(5788)
        |               `-php-fpm(5789)
        |-rsyslogd(1311)-+-{rsyslogd}(1312)
        |                |-{rsyslogd}(1313)
        |                `-{rsyslogd}(1314)
        |-sshd(1385)-+-sshd(1672)---bash(1676)
        |            |-sshd(1699)---bash(1703)---pstree(7120)
        |            `-sshd(5684)---bash(5688)
        `-udevd(516)-+-udevd(1543)
                     `-udevd(1544)

```

4.  ��ʵ��ģʽ������crontabʵ�ָ߿ɿ��Եġ���ʵ���ĺ�̨����
```php
#/wwwroot/beyosoft/multiworker/test.php�ļ��е�����
<?php
use beyosoft\cli\MulitWorker;
require __DIR__.'/MulitWorker.php';

$mp = new MulitWorker();
$mp->lockFile = '/var/run/multiworker.lock'; //����һ�����ļ� �ļ�������ʱ���Զ�����
$mp->workerNum = 6;
$mp->normalExitCode = 0;
$mp->onWorkerStart = function($pin) use($mp) {
    $mp->Log("$pin started .\n");
    sleep(3600);
    exit(0);
};

$mp->run();
```
crontab���������
```bash
*  *  *   *   *  /usr/local/php/bin/php /wwwroot/beyosoft/multiworker/test.php 2>&1 | /bin/logger
```
���������������ǽ���ʵ�������˳��������л����ٴ����У�����Ϊ���ļ��Ĵ��ڣ���Զֻ����һ��ʵ�����С�

