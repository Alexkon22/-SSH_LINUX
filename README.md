  ### команды для настройки  ssh   сервера 



# Настройка SSH-доступа

Добавление публичного SSH-ключа для пользователей `root` и `alex`, а также настройка безопасной аутентификации SSH-сервера.

## Подготовка

Замените `AAAAC...` на ваш актуальный публичный SSH-ключ.

---

## Для пользователя `root`

```bash
# Создание директории .ssh
mkdir -p /root/.ssh

# Установка прав на директорию
chmod 700 /root/.ssh

# Добавление публичного ключа
echo "ssh-ed25519 AAAAC..." >> /root/.ssh/authorized_keys

# Установка прав на файл с ключами
chmod 600 /root/.ssh/authorized_keys
```

**Путь к файлу:** `cd /root/.ssh/`

---

## Для пользователя `alex`

```bash
# Создание директории .ssh
mkdir -p /home/alex/.ssh

# Установка прав на директорию
chmod 700 /home/alex/.ssh

# Добавление публичного ключа
echo "ssh-ed25519 AAAAC..." >> /home/alex/.ssh/authorized_keys

# Установка прав на файл с ключами
chmod 600 /home/alex/.ssh/authorized_keys
```

**Путь к файлу:** `/home/alex/.ssh/authorized_keys`

---

## Настройка SSH-сервера

### 1. Открытие конфигурационного файла

```bash
sudo nano /etc/ssh/sshd_config
```

**Путь к файлу:** `/etc/ssh/sshd_config`

### 2. Настройка параметров аутентификации

Найдите строки, отвечающие за аутентификацию, и приведите их к такому виду (если строки закомментированы – раскомментируйте или добавьте новые):

```text
PasswordAuthentication no
ChallengeResponseAuthentication no
PermitRootLogin prohibit-password
```

**Пояснение:**

- `PasswordAuthentication no` – запрещает вход по паролю для всех пользователей.
- `ChallengeResponseAuthentication no` – отключает другие методы парольной аутентификации.
- `PermitRootLogin prohibit-password` – разрешает вход для root только по ключу (это безопаснее, чем `PermitRootLogin yes`).

Если у вас старая версия OpenSSH (до 7.0), используйте вместо последней строки:

```text
PermitRootLogin without-password
```

Сохраните файл (`Ctrl+O`, `Enter`, `Ctrl+X`).

### 3. Проверка синтаксиса конфигурации

Перед перезапуском убедитесь, что в конфиге нет ошибок:

```bash
sudo sshd -t
```

Если команда ничего не вывела – всё правильно. Если вывела ошибку – исправьте её (возможно, дублирующиеся строки или опечатки).

### 4. Перезапуск SSH-сервера

```bash
sudo systemctl restart sshd
```
### 5.Проверка  статуса firewalld:
```bash
systemctl status firewalld
```
**просмотр списка открытых портов**
```bash
firewall-cmd --list-ports
```
**открываем порт**
```bash
firewall-cmd --permanent --add-port=7070/tcp
firewall-cmd --reload
```
**Проверка что порт добавился**
```bash
firewall-cmd --list-ports
```
**если используется Ip Tables**
  проверка правил 
```bash
iptables -L -n -v | grep 7070
```
 если порт не разрешен добавьте правило 
 ```bash
iptables -I INPUT -p tcp --dport 7070 -j ACCEPT
service iptables save   # если используется служба iptables
systemctl restart iptables  # или сохранить через iptables-save
 ```
---

## Проверка настроек

```bash
# Проверка прав на директории
ls -la /root/.ssh
ls -la /home/alex/.ssh

# Проверка содержимого файлов
cat /root/.ssh/authorized_keys
cat /home/alex/.ssh/authorized_keys
```

---
## Проверка на Windows хостах
```ssh -i "C:\Users\ALEX\.ssh\bitrix_key" -p 7070 root@ip ```

--- 
## Примечания

- Права `700` на директорию `.ssh` — только владелец имеет доступ
- Права `600` на файл `authorized_keys` — только владелец может читать и записывать
- Формат ключа: `ssh-ed25519`
- После изменения `sshd_config` обязательно проверьте синтаксис и перезапустите SSH-сервер
- Не закрывайте текущую SSH-сессию до проверки нового подключения
