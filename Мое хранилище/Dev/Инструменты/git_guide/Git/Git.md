---
date: 28-04-2026
tags:
  - Git
status: editing
sticker: lucide//file-check
---
### Команды

### Настройки
`git config --global user.name` - выводит имя пользователя
`git config --global user.name <new_name>` - определяет новое имя пользователю
`git config --global user.email` - выводит почту пользователя
`git config --global user.email <new_email>` - определяет новую почту пользователя
`git config --global color.ui true` - включает цветного вывода информации
`git config core.editor <some_editor>` - устанавливает редактор по умолчанию
`git config --list` - выводит список всех настроек
`git config list --global` - выводит список глобальных настроек
`git config list --local` - выводит список локальных настроек
`git config list --show-origin --list` - выводит список всех настроек с информацией, из какого файла конфигурации каждая настройка взята

### Инициализация
`git init` - инициализация проекта (создает системную папку .git)
`git status` - определение статуса файлов
`git add <name_files>` - помечает файл как отслеживаемый git
`git add .` - помечает все файлы в папке как отслеживаемые git
`git rm --cached <name_file>` - делает файл не отслеживаемым git
`git commit -m "some comit"` - фиксация изменений.
`git remote add origin https://github.com/name/some-rep.git` - привязывает текущий репозиторий к удаленному
`git clone https://github.com/name/some-rep.git` - клонирование из удаленного репозитория

### Работа с ветками
`git branch` - посмотреть имеющиеся ветки
`git branch <name_new_branch>` - создание новой ветки
`git branch -D <name_delete_branch>` - удаление ветки
`git checkout <name_checkout_branch>` - переключение на другую ветку
`git checkout -b <name_new_branch>` - создание новой ветки и переключение на нее
`git merge <name_merge_branch>` - объединение текущей ветки с указанной

### Работа с коммитами
`git log` - просмотр всех коммитов
`git log --oneline` - просмотр всех коммитов в компактном виде
`git commit -a` - ленивый коммит
`git commit --amend` - добавляет изменения в индекс в предыдущий коммит
`git commit -m "add new message"` - добавляет коммит с указанным сообщением
`git push -u original main` - заливает из текущей ветки в удаленный репозиторий
`git pull` - вытягивание изменений из удаленного репозитория
`git reset` - отмена всего индекса (сохраняя изменения в папке)
`git reset <file_name>` - убирает файл из индекса
`git reset --hard` - отмена всех изменений после последнего коммита
`git reset --hard <file_name>` - убирает файл из указанного комита
`git reset HEAD~1` - перемещение головы на один коммит назад (число это количество коммитов)

![[reset.png]]

`git revert` - справка о команде
`git revert --help` - подробная справка о команде
`git revert HEAD` - откатывает последний коммит и создает новый коммит
`git revert HEAD --no-edit` - откатывает последний коммит без коммита
`git revert <id_commit>` - откатывает коммит по его идентификатору
`git revert --abort` - откатывает незавершенный откат
`git revert <id_commit>..<id_commit>` - откатывает серию коммитов
`git revert <id_commit> --no-commit` - откатывает без создания коммита
`git show` - показывает информацию о последнем коммите
`git show <commit_id>` - показывает информацию об указанном коммите
`git show --name-only <commit_id>` - показывает названия файлов в указанном коммите

![[Dev/Инструменты/git_guide/sourse/git_stages.png]]

### Функционал
`.gitignore` - файл, где определяются файлы, которые будут проигнорированы git