---
## Front matter
title: "Отчёта по лабораторной работе № 5"
subtitle: "Операционные системы"
author: "Сахно Алёна Юрьевна "

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы
Изучиь основные свойства структура базы паролей и уметь работать с pass.
# Задание

1. Научиться работать с Pass
# Теоретическое введение

Структура базы паролей

- Структура базы может быть произвольной, если Вы собираетесь использовать её напрямую, без промежуточного программного обеспечения. Тогда семантику структуры базы данных Вы держите в своей голове.
- Если же необходимо использовать дополнительное программное обеспечение, необходимо семантику заложить в структуру базы паролей.

Семантическая структура базы паролей

- Рассмотрим пользователя user в домене example.com, порт 22.

- Отсутствие имени пользователя или порта в имени файла означает, что любое имя пользователя и порт будут совпадать:

    example.com.pgp

- Соответствующее имя пользователя может быть именем файла внутри каталога, имя которого совпадает с хостом. Это полезно, если в базе есть пароли для нескольких пользователей на одном хосте:

    example.com/user.pgp

- Имя пользователя также может быть записано в виде префикса, отделенного от хоста знаком @:

    user@example.com.pgp

- Соответствующий порт может быть указан после хоста, отделённый двоеточием (:):

    example.com:22.pgp
    example.com:22/user.pgp
    user@example.com:22.pgp

- Эти все записи могут быть расположены в произвольных каталогах, задающих Вашу собственную иерархию.

Способы создания файла шаблона

- При первом добавлении файла передайте аргумент --template:

    chezmoi add --template ~/.zshrc

- Если файл уже контролируется chezmoi, но не является шаблоном, можно сделать его шаблоном:

    chezmoi chattr +template ~/.zshrc

- Можно создать шаблон вручную в исходном каталоге, присвоив ему расширение .tmpl:

    chezmoi cd
    $EDITOR dot_zshrc.tmpl

- Шаблоны в каталоге .chezmoitemplates должны создаваться вручную:

    chezmoi cd
    mkdir -p .chezmoitemplates
    cd .chezmoitemplates
    $EDITOR mytemplate

Редактирование файла шаблона

- Используйте chezmoi edit:

    chezmoi edit ~/.zshrc

- Чтобы сделанные вами изменения сразу же применялись после выхода из редактора, используйте опцию --apply:

    chezmoi edit --apply ~/.zshrc


# Выполнение лабораторной работы

Менеджер паролей pass

Установка

    Fedora

        pass; dnf install gopass


 (рис. [-@fig:001]).

![Установка](image/1.jpg){#fig:001 width=70%}

Настройка

    Ключи GPG

        Просмотр списка ключей:

        gpg --list-secret-keys

        Если ключа нет, нужно создать новый:

        gpg --full-generate-key

(рис. [-@fig:002]).

![Настройка](image/2.jpg){#fig:002 width=70%}


Инициализация хранилища

    Инициализируем хранилище:

    pass init <gpg-id or email>


(рис. [-@fig:003]).

![Инциаоизация хранилища](image/3.jpg){#fig:003 width=70%}

Синхронизация с git

-  Создадим структуру git:

        pass git init

- Также можно задать адрес репозитория на хостинге (репозиторий необходимо предварительно создать):

        pass git remote add origin git@github.com:<git_username>/<git_repo>.git

(рис. [-@fig:004]).

![Инциаоизация хранилища](image/4.jpg){#fig:004 width=70%}

Для синхронизации выполняется следующая команда:

        pass git pull
        pass git push

Прямые изменения
- Следует заметить, что отслеживаются только изменения, сделанные через сам gopass (или pass).
- Если изменения сделаны непосредственно на файловой системе, необходимо вручную закоммитить и выложить изменения:

            cd ~/.password-store/
            git add .
            git commit -am 'edit manually'
            git push

(рис. [-@fig:005]).

![Прямые изменения](image/5.jpg){#fig:005 width=70%}

- Проверить статус синхронизации модно командой

            pass git status

Настройка интерфейса с броузером

- Для взаимодействия с броузером используется интерфейс native messaging.
- Поэтому кроме плагина к броузеру устанавливается программа, обеспечивающая интерфейс native messaging.

    Плагин browserpass
            Fedora

            dnf copr enable maximbaz/browserpass
            dnf install browserpass


(рис. [-@fig:006]).

![Настройка интерфейса с броузером](image/6.jpg){#fig:006 width=70%}

Сохранение пароля

- Добавить новый пароль

        Выполните:

        pass insert [OPTIONAL DIR]/[FILENAME]

            OPTIONAL DIR: необязательное имя каталога, определяющее файловую структуру для вашего хранилища паролей;
            FILENAME: имя файла, который будет использоваться для хранения пароля.

        Отобразите пароль для указанного имени файла:

        pass [OPTIONAL DIR]/[FILENAME]

        Замените существующий пароль:

        pass generate --in-place FILENAME


(рис. [-@fig:007]).

![Сохранение пароля ](image/7.jpg){#fig:007 width=70%}

Управление файлами конфигурации

Дополнительное программное обеспечение

    Установите дополнительное программное обеспечение:

    sudo dnf -y install \
             dunst \
             fontawesome-fonts \
             powerline-fonts \
             light \
             fuzzel \
             swaylock \
             kitty \
             waybar swaybg \
             wl-clipboard \
             mpv \
             grim \
             slurp



(рис. [-@fig:008]).

![Управление файлами конфигурации](image/8.jpg){#fig:008 width=70%}
   
 Установите шрифты:

    sudo dnf copr enable peterwu/iosevka
    sudo dnf search iosevka
    sudo dnf install iosevka-fonts iosevka-aile-fonts iosevka-curly-fonts iosevka-slab-fonts iosevka-etoile-fonts iosevka-term-fonts

(рис. [-@fig:009]).

![Установите шифр ](image/9.jpg){#fig:009 width=70%}

Установка

    Установка бинарного файла. Скрипт определяет архитектуру процессора и операционную систему и скачивает необходимый файл:

        с помощью wget:

        sh -c "$(wget -qO- chezmoi.io/get)"

(рис. [-@fig:010]).

![Установка ](image/10.jpg){#fig:010 width=70%}

Создание собственного репозитория с помощью утилит

    Будем использовать утилиты командной строки для работы с github.

    Создадим свой репозиторий для конфигурационных файлов на основе шаблона:

    gh repo create dotfiles --template="yamadharma/dotfiles-template" --private

(рис. [-@fig:005]).

![Создание собственного репозитория с помощью утилит ](image/11.jpg){#fig:011 width=70%}

Подключение репозитория к своей системе

    Инициализируйте chezmoi с вашим репозиторием dotfiles:

    chezmoi init git@github.com:<username>/dotfiles.git

    Проверьте, какие изменения внесёт chezmoi в домашний каталог, запустив:

    chezmoi diff

    Если вас устраивают изменения, внесённые chezmoi, запустите:

    chezmoi apply -v

(рис. [-@fig:012]).

![Подключение репозитория к своей системе ](image/12.jpg){#fig:012 width=70%}

Использование chezmoi на нескольких машинах

    На второй машине инициализируйте chezmoi с вашим репозиторием dotfiles:

    chezmoi init https://github.com/<username>/dotfiles.git

    Или через ssh:

    chezmoi init git@github.com:<username>/dotfiles.git

    Проверьте, какие изменения внесёт chezmoi в домашний каталог, запустив:

    chezmoi diff

    Если вас устраивают изменения, внесённые chezmoi, запустите:

    chezmoi apply -v

    Если вас не устраивают изменения в файле, отредактируйте его с помощью:

    chezmoi edit file_name

    Также можно вызвать инструмент слияния, чтобы объединить изменения между текущим содержимым файла, файлом в вашей рабочей копии и измененным содержимым файла:

    chezmoi merge file_name

    При существующем каталоге chezmoi можно получить и применить последние изменения из вашего репозитория:

    chezmoi update -v

(рис. [-@fig:0013]).

![На второй машине ](image/13.jpg){#fig:013  width=70%}

Настройка новой машины с помощью одной команды

    Можно установить свои dotfiles на новый компьютер с помощью одной команды:

    chezmoi init --apply https://github.com/<username>/dotfiles.git

    Через ssh:

    chezmoi init --apply git@github.com:<username>/dotfiles.git

Ежедневные операции c chezmoi

    Извлеките последние изменения из репозитория и примените их

        Можно извлечь изменения из репозитория и применить их одной командой:

        chezmoi update

        Это запускается git pull --autostash --rebase в вашем исходном каталоге, а затем chezmoi apply.

-  Извлеките последние изменения из своего репозитория и посмотрите, что изменится, фактически не применяя изменения

        Выполните:

        chezmoi git pull -- --autostash --rebase && chezmoi diff

Это запускается git pull --autostash --rebase в вашем исходном каталоге, а chezmoi diff затем показывает разницу между целевым состоянием, вычисленным из вашего исходного каталога, и фактическим состоянием.

- Если вы довольны изменениями, вы можете применить их:

        chezmoi apply


(рис. [-@fig:014]).

![Ежедневные операциир ](image/14.jpg){#fig:014 width=70%}

Автоматически фиксируйте и отправляйте изменения в репозиторий
   Можно автоматически фиксировать и отправлять изменения в исходный каталог в репозиторий.
   Эта функция отключена по умолчанию.

- Чтобы включить её, добавьте в файл конфигурации ~/.config/chezmoi/chezmoi.toml следующее:

        [git]
            autoCommit = true
            autoPush = true

- Всякий раз, когда в исходный каталог вносятся изменения, chezmoi фиксирует изменения с помощью автоматически сгенерированного сообщения фиксации и отправляет их в ваш репозиторий.
- Будьте осторожны при использовании autoPush. Если ваш репозиторий dotfiles является общедоступным, и вы случайно добавили секрет в виде обычного текста, этот секрет будет отправлен в ваш общедоступный репозиторий.

(рис. [-@fig:015]).

![Изменение в репозиторий](image/15.jpg){#fig:015 width=70%}

# Выводы

Я изучила основные свойства структура базы паролей и сумела по- работать с pass.

# Список литературы{.unnumbered}

https://esystem.rudn.ru/mod/page/view.php?id=1224377
