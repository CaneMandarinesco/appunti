https://www.ilpuntotecnico.com/sblocco-smart-modem-tim-telecomitalia-technicolor-tg789-vac/

### modificare la configurazione del modem
comandi per caricare la configurazione custom
```bash
# decripta il file .ini
openssl aes-128-cbc -K a875e62aa6f1d430dac45fcd0e3bb246 -nosalt -iv 0 -d -in user_config.ini -out user.ini
```

contenuto da modificare in `user.ini`:
```ini
# aggiungere i seguenti utenti nella sezione mpluser.ini (linea 1090 circa)
[ mlpuser.ini ]
add
name=Unlock
password=_CYP2_53e2ac63e7b839c0708ec54912d3a36c87d6655ade7089b8
role=BHSAdmin hash2=d77b2bd8ab60bf664ac7d6151215b9f4 crypt=GiD5FRyGAVFGI


add name=R00t
password=_CYP2_53e2ac63e7b839c0708ec54912d3a36c87d6655ade7089b8 role=ALL
hash2=d77b2bd8ab60bf664ac7d6151215b9f4 crypt=GiD5FRyGAVFGI 
```

modificare da `disabled` a `enabled` le seguenti proprieta'
```ini
modify name=FTP state=disabled natpmweight=10
modify name=SSH state=enabled natpmweight=10
modify name=TELNET state=disabled natpmweight=10
```

ora localizzate il seguente testo e modificate da `mode=full` a `mode=read-only`:
```ini
[ cwmp.ini ]
config state=enabled mode=full periodicInform=disabled periodicInfInt=43200 sessionTimeout=60 noIpTimeout=180 maxEnvelopes=1 connectionRequest=enabled connectionReqAuth=digest bootdelayrange=0 persistentSubscription=enabled connectionReqThrotNumber=1 connectionReqThrotTime=0 activeNotifThrotTime=0 maxWriteBufferSize=4096 showPasswords=enabled
```

> quest'ultima modifica permette al tuo isp di accedere al tuo router solo in modalita' di lettura, impedendo la scrittura di aggiornamenti sul dispostivo.

### salvare la configurazione 
ora bisogna criptare il file:
```
openssl aes-128-cbc -K a875e62aa6f1d430dac45fcd0e3bb246 -nosalt -iv 0 -e -in user.ini -out user_config.ini
```

e infine possiamo caricarlo dalla pagina del router.  

usando il protocollo ftp scrivamo il file security.cfg


```
# ssh 
ssh -o KexAlgorithms=diffie-hellman-group1-sha1 -o HostKeyAlgorithms=ssh-dss -o Ciphers=3des-cbc  Unlock@192.168.1.1
```

cfw: https://hack-technicolor.readthedocs.io/en/latest/Repository/#tg789vac-v2-vant-6 inet
https://github.com/BoLaMN/tch-exploit/releases/tag/2.0.1-rc7