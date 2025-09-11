## *Answers to questions in part A:*

## Ответ 1:
**Commit** создает новый коммит, а **amend** не создает новый. Amend изменяет последний коммит(заголовок, код и тд).
## Ответ 2:
git branch **-d** безопасно удаляет ветку, но только если ее данные полностью закинуты в другую ветку. А **-D** принудительно удаляет ветку.    
## Ответ 3: 
git checkout branch - HEAD на ветку. git checkout commit - HEAD на коммит. detached HEAD он указывает напрямую на коммит, а не на ветку.
## Ответ 4:
после git reset **--soft** сделает шаг назад, но сохранит изменения в индексе которого reset. git reset **--mixed** используется по умолчанию. **--hard** сделает шаг назад и очистит индекс, и удалит файлы в рабочей областе.
## Ответ 5:
**git revert** создает новый коммит, который отменяет пред. коммит. **git reset** отменяет изменения в рабочей области и переходит к определенному коммиту(все коммиты после него - GG).
## Ответ 6: 
**git merge --no-ff** создает коммит слияния при обьединении веток, очень удобно чтобы сохранить историю основной ветки. Когда в нашей не основной ветке было сделано много мини коммитов можно удобно слить с помощью **--squash**.
## Ответ 7:
при **git rebase name** происходит перемещение из checkout ветки на конец другой ветки. Чтобы прервать rebase напиши git rebase --abort и все.
## Ответ 8:
Для начала надо перейти на ветку(git checkout) в которую ты хочешь скопировать эти коммиты, а потом написать git cherry-pick 1c 2c 3c.
**-x** - добавляет хэш коммита в сообщение коммита.
**-e** - редактирование сообщения коммита.
**-n** - изменения из коммита где ты сейчас находишься будут применемы в рабочую область. 
## Ответ 9:
**git cherry-pick --abort**
**git merge --abort**
**git rebase --abort**
## Ответ 10:
К примеру я слил 2 ветки и появился доп.коммит merge-commit 5465 в котором находятся все данные коммитов 2 веток. 
**git revert -m**, служит для отмены этого мердж коммита после -m пишем цифру родителя коммита можно 1 поставить к примеру это main в который вливали и после 1 пишем хэш коммита мердж который следует отменить. 

mkdir git-selftest && cd git-selftest<br>
git init<br>
git checkout -b main<br>

printf "Hello\n" > app.txt<br>
git add app.txt<br>
git commit -m "c1: initial app"<br>
git tag c1<br>

printf "Hello from main\n" > app.txt<br>
git commit -am "c2: tweak greeting on main"<br>
git tag c2<br>

git checkout -b feature/menu c1<br>
printf "Hello from menu\n" > app.txt<br>
git commit -am "m1: menu greeting"<br>
git tag m1<br>

git checkout -b feature/login c2<br>
printf "login form\n" >> app.txt<br>
git commit -am "f1: add login block"<br>

echo "client-side validation" >> app.txt<br>
git commit -am "f2: login validation"<br>
git tag f2<br>

git checkout -b feature/promo c1<br>
printf "promo banner\n" >> app.txt<br>
git commit -am "p1: add promo banner"<br>
git tag p1<br>

git checkout main<br>

git log --oneline --graph --decorate --all<br>
touch answers.md<br>
git add answers.md<br>
git commit -m "A: answers to questions"<br>
git tag a1<br>

git checkout -b feature/header<br>
git branch -m feature/header feat/header<br>
git branch -d feature/header<br>
- error: branch 'feature/header' not found<br>

git branch -vv<br>

| Ветки         | Коммиты/Теги/Названия          |
|---------------|--------------------------------|
| feat/header   | 9e004ad A: answers to questions|
| feature/login | cecd402 f2: login validation   |
| feature/menu  | d477c7b m1: menu greeting      |
| feature/promo | b649225 p1: add promo banner   |
| main          | 9e004ad A: answers to questions|


git checkout feat/header<br>
echo "header section" >> app.txt<br>
git add .<br>
git commit -m "h1: add header"<br>
git log --oneline --graph --decorate --all<br>
git tag h1<br>

git checkout main<br>
echo "footer section" >> app.txt<br>
git add .<br>
git commit -m "c3: add footer"<br>
git log --oneline --decorate --graph --all -- app.txt<br>

git checkout main<br>
git merge feat/header --no-ff<br>
Auto-merging app.txt<br>
CONFLICT (content): Merge conflict in app.txt<br>
Automatic merge failed; fix conflicts and then commit the result.<br>
Вызвался конфликт и я принял Оба изменения.<br>
git add app.txt<br>
git commit -m "merge: bring header"<br>
git log --oneline --graph --decorate <br>

Был конфликт решил его приняв входящие изменения.<br>
git revert 752d <br>
git add .<br>
git revert --continue <br>

git reset --hard HEAD^<br>
git reflog<br>
git reset --hard 087d (HEAD is now at 087dcc8 Revert "revert: remove footer")<br>
087dcc8 (HEAD -> main) HEAD@{0}: reset: moving to 087d<br>
112eed5 HEAD@{1}: reset: moving to HEAD^<br>
087dcc8 (HEAD -> main) HEAD@{2}: commit (amend): Revert "revert: remove footer"<br>
797962e HEAD@{3}: commit: Revert "c3: add footer"<br>
112eed5 HEAD@{4}: checkout: moving from main to main<br>
112eed5 HEAD@{5}: checkout: moving from main to main<br>
112eed5 HEAD@{6}: commit (merge): merge: bring header<br>
752d0e7 HEAD@{7}: checkout: moving from main to main<br>
752d0e7 HEAD@{8}: commit: c3: add footer<br>
9e004ad (tag: a1) HEAD@{9}: checkout: moving from feat/header to main<br>

* 087dcc8 (HEAD -> main) Revert "revert: remove footer"<br>
*   112eed5 merge: bring header<br>
|\<br>
| * 7f0b835 (tag: h1, feat/header) h1: add header<br>
* | 752d0e7 c3: add footer<br>
|/<br>
* 9e004ad (tag: a1) A: answers to questions<br>
* dc00c79 (tag: c2) c2: tweak greeting on main<br>
* 0d5435d (tag: c1) c1: initial app<br>

git checkout main<br>
$ git merge feature/menu<br>
Auto-merging app.txt<br>
CONFLICT (content): Merge conflict in app.txt<br>
Automatic merge failed; fix conflicts and then commit the result.<br>
git add .<br>
git commit -m "merge: accept menu greeting"<br>
cat app.txt<br>
Hello from main<br>
menu greeting accepted<br>

git checkout feature/promo<br>
git cherry-pick f2~1..f2<br>
Auto-merging app.txt<br>
CONFLICT (content): Merge conflict in app.txt<br>
error: could not apply cecd402... f2: login validation<br>
git add .<br>
git cherry-pick --continue<br>

git log --oneline --decorate --graph feature/promo<br>
git rebase -i --root<br>
59624bc (HEAD -> feature/promo) promo: login + validation<br>

git checkout feature/login<br>
git rebase --onto main c2<br>
git add .<br>
git rebase --continue<br>
[detached HEAD 1d5528a] f2: login validation<br>
 1 file changed, 3 insertions(+), 1 deletion(-)<br>
Successfully rebased and updated refs/heads/feature/login.<br>

git checkout main<br>
git add .<br>
git commit -m "fix: urgent patch." // Добавил 3 строку в app.txt<br>
git checkout feature/promo <br>
git log --all --oneline<br>
git cherry-pick cd322 // Конфликт - решил <br>
git cherry-pick --continue<br>
[feature/promo 021601a] fix: urgent patch.<br>
Date: Thu Sep 11 18:17:16 2025 +0300<br>
1 file changed, 2 insertions(+), 3 deletions(-)<br>
git revert cd322 // Revert "fix: urgent patch."<br>
git log --oneline --graph --decorate --all<br>

git status<br>
On branch main<br>
nothing to commit, working tree clean<br>

Коммит для добавления последовательности команд (B-D):<br>
git checkout main<br>
git add answers.md / Снизу<br>
git commit -m "answers update"<br>
