# mmd_launchpad

## Deploy nuances

* в database.yml host указываем название сервиса базы данных

* генерируем ключи с помощью rails credentials:edit --environment staging
* scp копируем приватный и йамл ключи на сервер, пусть там лежит всегда

## SSH

scp .env root@185.221.213.49:/home/aynur/mmd_launchpad

I have an error on my server:
It turned out that  ssh-agent is not started automatically when I log in.

```shell
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

> ssh-agent bash
root@host1866467-1:/home/aynur/
> ssh-add ~/.ssh/2023_12_15_github
Identity added: /root/.ssh/2023_12_15_github (aykuli@ya.ru)
```

ssh-agent - программа, которая отслеживает идентификационный ключи пользователя и их passphrases.

Обычно по умолчанию есть авто включение ssh-agent, но бывает и нет.
Проверьте переменную окружения SSH_AGENT_SOCK:

```shell
echo $SSH_AGENT_SOCK
```

У меня нет этой перменной, однако работает.
Также для вклчения возможности аутентификации через ssh-ключи, должно быть вклчена возможноть [`SSH Public Key Authentication`](https://www.ssh.com/academy/ssh/public-key-authentication).

## Backup DB

On local machine:

```shell
ssh mmd 'docker exec mmd_db pg_dump -U yourpersonalmanager --dbname lifelongdata > /home/aynur/lifelongdata_backup_`date +%d_%m_%Y`.sql'

scp mmd:/home/aynur/lifelongdata_backup_04_01_2024.sql .
```

## Resources

1. <https://web.archive.org/web/20210506080335/https://mah.everybody.org/docs/ssh>
2. [ssh-agent: How to configure ssh-agent, agent forwarding, & agent protocol](https://www.ssh.com/academy/ssh/agent)
3. [SSH Public Key Authentication](https://www.ssh.com/academy/ssh/public-key-authentication)
