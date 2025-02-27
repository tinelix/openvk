# <img align="right" src="https://github.com/openvk/openvk/raw/master/Web/static/img/logo_shadow.png" alt="openvk" title="openvk" width="15%">Tinelix Astorium

_[English](README.md)_

_**Tinelix Astorium** - форк, основанный на [OSS-проекте OpenVK](https://github.com/openvk/openvk) и разработан для своих нужд и потребностей._

**OpenVK** — это попытка создать простую CMS, которая ~~косплеит~~ имитирует старый ВКонтакте. На данный момент, представленный здесь исходный код проекта пока не является стабильным.

ВКонтакте принадлежит Павлу Дурову и VK Group.

Честно говоря, мы даже не знаем, работает ли она вообще. Однако, эта версия поддерживается, и мы будем рады принять ваши сообщения об ошибках [в нашем баг-трекере](https://github.com/openvk/openvk/projects/1). Вы также можете отправлять их через [вкладку "Помощь"](https://openvk.su/support?act=new) (для этого вам понадобится учетная запись OpenVK).

## Когда выйдет релизная версия?

Мы выпустим OpenVK, как только он будет готов. На данный момент Вы можете:
* Склонировать master ветку репозитория командой `git clone` (используйте `git pull` для обновления)
* Взять готовую сборку OpenVK из [GitHub Actions](https://nightly.link/openvk/archive/workflows/nightly/master/OpenVK%20Archive.zip)

## Инстанции

* **[openvk.su](https://openvk.su/)**
  * **[openvk.uk](https://openvk.uk)** ([зеркало](<https://t.me/openvk/1609>))
  * **[openvk.co](http://openvk.co)** (зеркало [без TLS](<https://t.me/openvk/1654>))
* [social.fetbuk.ru](http://social.fetbuk.ru/)
* [vepurovk.xyz](http://vepurovk.xyz/)
  * **[vepurovk.fun](http://vepurovk.fun)** (зеркало без TLS)
* [ovk.tinelix.ru](https://ovk.tinelix.ru)
  * [зеркало без TLS](http://ovk.tinelix.ru)

## Могу ли я создать свою собственную инстанцию Tinelix Astorium / OpenVK?

Да! И всегда пожалуйста.

Однако, Tinelix Astorium использует Chandler Application Server. Это программное обеспечение требует расширений, которые могут быть не предоставлены вашим хостинг-провайдером (а именно, sodium и yaml. Эти расширения доступны на большинстве хостингов ISPManager).

Если хотите, вы можете добавить вашу инстанцию в список выше, чтобы люди могли зарегистрироваться там.

### Процедура установки

1. Установите PHP 7.4, веб-сервер, Composer, Node.js 10+, Yarn и [Chandler](https://github.com/openvk/chandler)

   _Для граждан РФ крайне важен [переход Composer на аполитический репозиторий](https://stackoverflow.com/a/76131122)._

* PHP 8 пока ещё тестируется, работоспособность движка на этой версии PHP пока не гарантируется.

2. Установите MySQL-совместимую базу данных.

* Мы рекомендуем использовать Persona Server, но любая MySQL-совместимая база данных должна работать.
* Сервер должен поддерживать хотя бы MySQL 5.6, рекомендуется использовать MySQL 8.0+.
* Поддержка для MySQL 4.1+ находится в процессе, а пока замените `utf8mb4` и `utf8mb4_unicode_520_ci` на `utf8` и `utf8_unicode_ci` в SQL-файлах, соответственно.

3. Установите [commitcaptcha](https://github.com/openvk/commitcaptcha) и OpenVK в качестве расширений Chandler:

```bash
git clone https://github.com/tinelix/astorium /path/to/chandler/extensions/available/openvk
git clone https://github.com/openvk/commitcaptcha /path/to/chandler/extensions/available/commitcaptcha
```

4. И включите их:

```bash
ln -s /path/to/chandler/extensions/available/commitcaptcha /path/to/chandler/extensions/enabled/
ln -s /path/to/chandler/extensions/available/openvk /path/to/chandler/extensions/enabled/
```

5. Импортируйте `install/init-static-db.sql` в **ту же базу данных**, в которую вы установили Chandler, и импортируйте все SQL файлы из папки `install/sqls` в **ту же базу данных**
6. Импортируйте `install/init-event-db.sql` в **отдельную базу данных** (Яндекс.Clickhouse также может быть использован, настоятельно рекомендуется)
7. Скопируйте `openvk-example.yml` в `openvk.yml` и измените параметры под свои нужды
8. Запустите `composer install` в директории OpenVK
9. Запустите `composer install` в директории commitcaptcha
10. Перейдите в `Web/static/js` и выполните `yarn install`
11. Установите `openvk` в качестве корневого приложения в файле `chandler.yml`
12. Выставите права во всех директориях `chandler` на 0777 (все права на чтение, запись и выполнение), если отличается

После этого вы можете войти как системный администратор в саму сеть (регистрация не требуется):

* **Логин**: `admin@localhost.localdomain6`
* **Пароль**: `admin`
  * Перед использованием встроенной учетной записи настоятельно рекомендуется сменить пароль или отключить её.

💡Запутались? Полное руководство по установке доступно [здесь](https://docs.openvk.uk/openvk_engine/centos8_installation/) (CentOS 8 [и](https://almalinux.org/ru/) [семейство](https://yum.oracle.com/oracle-linux-isos.html)).

# Установка в Docker/Kubernetes
Подробные иструкции можно найти в `install/automated/docker/README.md` и `install/automated/kubernetes/README.md` соответственно.

### Если мой сайт использует Tinelix Astorium / OpenVK, должен ли я публиковать его исходные тексты?

Это зависит от обстоятельств. Вы можете оставить исходные тексты при себе, если не планируете распространять бинарники вашего сайта. Если программное обеспечение вашего сайта должно распространяться, оно может оставаться не-OSS при условии, что OpenVK не используется в качестве основного приложения и не модифицируется. Если вы модифицировали OpenVK для своих нужд или ваша работа основана на нем и вы планируете ее распространять, то вы должны лицензировать ее на условиях любой совместимой с LGPL лицензии (например, OSL, GPL, LGPL и т.д.).

## Где я могу получить помощь?

Вы можете связаться с нами через:

* [Баг-трекер](https://github.com/openvk/openvk/projects/1)
* [Помощь в OVK](https://openvk.su/support?act=new)
* Telegram-чат: Перейдите на [наш канал](https://t.me/openvk) и откройте обсуждение в меню нашего канала.
* [Reddit](https://www.reddit.com/r/openvk/)
* [GitHub Discussions](https://github.com/openvk/openvk/discussions)
* Чат в Matrix: #ovk:matrix.org

**Внимание**: баг-трекер, форум, Telegram- и Matrix-чат являются публичными местами, и жалобы в OVK обслуживается волонтерами. Если вам нужно сообщить о чем-то, что не должно быть раскрыто широкой публике (например, сообщение об уязвимости), пожалуйста, свяжитесь с нами напрямую по этому адресу: **openvk [собачка] tutanota [точка] com**.

## ДИСКЛЕЙМЕР
OpenVK и Tinelix Astorium никак не связаны с компанией ООО "ВК" или не одобрены ею. 
