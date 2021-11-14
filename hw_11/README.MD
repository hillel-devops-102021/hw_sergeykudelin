# Практика работы с Git и GitHub

## Конфигурация клиента

- устанавливаем git по ссылке https://git-scm.com/download
- Создаем пустой каталог для домашних работ
- Переходи в него и инициализурет git проект.
```
    git init
```
- конфигурирует пользователя под которым будем работать
```
    # если у вас несколько аккаунтов, рекоммендуется назначать пользователя индивидуально для каждого проекта, без ключа --global
    git config --global user.name "Vasil Chumak"
    git config --global user.email vasil.chumak@gmail.com
    # email должен совпадать с вашей учетной записью GitHub, для проверки полномочий на доступ в проект
```
- Создадим каталог hw_11 и будет выполнять все действия в рамках данного каталога

## Базовые действия

- Cоздаем файл
```
    echo "First line in project" >> file.txt
    git status # смотрит статус untracked
```
- Добавляем в index
```
    git add file.txt
    git status # смотрим статус staged
    git diff --cached # смотрим проиндексированные изменения
```
- Добавляем изменения в историю
```
    git commit -m "First commit"
    git log # смотрим историю коммитов
    git show # смотрим информацию о коммите
```
- Добавляем новые изменения
```
    echo "Odessa is a city in the south of the country" >> file.txt
    git diff # непроиндексированные изменения, сравните с git diff --cached
    git commit -am "Info about Odessa" # удалось ли нам сделать комит без добавления в Index или в Index мы все же добавляем изменения перед комитом?
    git log # проверяем в истории, сделали ли мы комит
    # Попробуем обновить коммит, дополнительными действиями
    echo "Kiev is a city in the north of the country" >> file.txt
    git add file.txt
    git commit --amend # поменяйте сообщение "I like Kiev same as Odessa too"
    git log # проверяем кол-во коммитов
    git show # смотрим содержание последнего коммита
```
## Самостоятельно отработайте следующее

- "e" в конец файла и сохраните с комментарием в коммите "Info about Lviv"

## Откат действий

- Возьмите hash 2 коммита из git log и откатите изменения из него (example)
```
    git revert 5re2sf3 # Рекомендуется ипользовать первые 7 символов или HEAD~1
```
- Должен возникнуть конфликт из-за ранее созданого 3 коммита
```
    ➜  hw_11 git:(master) git revert HEAD~1
    Auto-merging hw_11/file.txt
    CONFLICT (content): Merge conflict in hw_11/file.txt
    error: could not revert 2de53e1... I like Kiev same as Odessa too:
    hint: after resolving the conflicts, mark the corrected paths
    hint: with 'git add <paths>' or 'git rm <paths>'
    hint: and commit the result with 'git commit'
```
- Решаем конфликт
```
    git status
    cat file.txt
    First line in project
    <<<<<<< HEAD
    Odessa is a city in the south of the country
    Kiev is a city in the north of the country
    Lviv is a city in the east of the country
    =======
    >>>>>>> parent of bb14131 (I like Kiev same as Odessa too)
    git show HEAD~1 # смотрим какие строчки были добавлены  в данном коммите
```
- Убираем строчки добавленные в рамках того коммита

```
    First line in project
    <<<<<<< HEAD
    Lviv is a city in the east of the country
    =======
    >>>>>>> parent of bb14131 (Info about Odessa)
```
- Убираем технические поля
```
    First line in project
    Lviv is a city in the east of the country
```
- Добавляем изменения в index
```
    git add file.txt
    git revert --continue
    git log # Как мы видим, revert не меняет историю, а вносит изменения относительно коммитами который мы захотели убрать.
    git show
```
## Работа с gitignore
- Создаем временный файл
```
    echo "this is a garbage file" | tee garbage1.tmp garbage2.tmp
    git status
```
- Создайте gitignore и добавьте данные файл в исключения, через фильтр по расширению
```
    echo "*.tmp" >> .gitignore
```
- Проверьте их отсутствие для добавления в index
```
    git status
```
- Добавьте Ваш gitignore в index и создайте коммит для его сохранения.
```
    git add .gitignore
    git commit -m "tmp to gitignore"
```

## Index
- Попробуйте воссоздать случай когда файл file.txt находиться в двух состояниях одновременно
```
➜  sergeykudelin_hw git:(master) ✗ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   file.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   file.txt
```
- Исправьте ситуация за счет добавления всех изменений в коммит, со следующим состоянием, то есть должен появится еще один коммит с произвольным описание к примеру "Info about Donetsk"
```
➜  new_tmp git:(master) git status
On branch master
nothing to commit, working tree clean
```

## Работа с ветками

- Создаем ветки
```
    git branch first # создает ветку
    git branch # смотрим локальные ветки
    git checkout first # переключаемся
    git checkout -b second # создаем дополнительную и переходим в ее с использованием одной команды
    git status # проверяем текущую ветку
    git branch # смотрим локальные ветки
```
- Внесем изменения в текущую ветку
```
    echo "Let's change the branch" > branch.txt
```
- Закомитьте данные правки с комментарием "Branch has been changed"
- Переключаемся на первую ветку
```
    git checkout first
    echo "And this branch too" > first.txt
    git add first.txt
    git commit -m "First branch has been changed too"
```
- Строим граф изменений
```
    git log --all --decorate --oneline --graph
    git config --global alias.g 'log --all --decorate --oneline --graph' #создадим alias
    git g
```
- Попробуйте выполнить!
- Проведем слияние веток
```
    git checkout master
    ls
    git merge first
    ls
    git g # merge прозошел без создания дополнительного коммита
    git checkout second
    git rebase master
    git g # история ветки изменилась
```
- Удалем ветку first
```
    git branch -d first
    git branch # смотрим оставшиеся ветки
```
- Прекращаем действия с данным репозиторием

## Отправляем изменения в origin

- Добавляем remote в Ваш текущий локальный проект
```
    git remote add origin git@github.com:hillel-devops-102021/hw_<YourUserName>.git # где <YourUserName> - имя Вашего GitHub аккаунта
```
- Отправим все ветки в remote
```
    git push origin -u --all
```

## Создаем PR с ветки second в master

- Самостоятельно (https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- Добавляем в sergeykudelin в Reviewers

## Отправка на проверку

- Линк на PR указываем описание домашнего задания и все.

## В качестве бонуса (ПОКА НЕ ОБЯЗАТЕЛЬНО)

- Хорошей практикой является создание описания чего либо Вами сделанного в формате MarkDown в файле README.MD
- README.MD может быть общий в корне проекта или индивидуальным в каталогах
- Создайте README.MD с описание Ваших действий в подкаталоге hw_11.
