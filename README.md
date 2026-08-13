  ### команды для настройки  ssh   сервера 



 # Настройка SSH-доступа

Добавление публичного SSH-ключа для пользователей `root` и `alex`.

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

**Путь к файлу:** `/root/.ssh/authorized_keys`

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

## Проверка настроек

```bash
# Проверка прав на директории
ls -la /root/.ssh
ls -la /home/alex/.ssh

# Проверка содержимого файлов
cat /root/.ssh/authorized_keys
cat /home/alex/.ssh/authorized_keys
```

## Примечания

- Права `700` на директорию `.ssh` — только владелец имеет доступ
- Права `600` на файл `authorized_keys` — только владелец может читать и записывать
- Формат ключа: `ssh-ed25519`
