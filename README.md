# mmd_launchpad

## Deploy nuances

* в database.yml host указываем название сервиса базы данных

* генерируем ключи с помощью rails credentials:edit --environment staging
* scp копируем приватный и йамл ключи на сервер, пусть там лежит всегда

## SSH

scp .env root@addr:/home/user/directory

I have an error on my server:
It turned out that  ssh-agent is not started automatically when I log in.

```shell
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

> ssh-agent bash
root@host1866467-1:/home/user/
> ssh-add ~/.ssh/pub_key_name
Identity added: /root/.ssh/pub_key_name (aykuli@ya.ru)
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
ssh mmd 'docker exec container_name pg_dump -U yourpersonalmanager --dbname somedata > /home/user/data_backup_`date +%d_%m_%Y`.sql'

scp mmd:/home/aynur/backup.sql .
```

## Resources

1. <https://web.archive.org/web/20210506080335/https://mah.everybody.org/docs/ssh>
2. [ssh-agent: How to configure ssh-agent, agent forwarding, & agent protocol](https://www.ssh.com/academy/ssh/agent)
3. [SSH Public Key Authentication](https://www.ssh.com/academy/ssh/public-key-authentication)
