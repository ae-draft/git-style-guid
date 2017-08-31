# Git Style Guide

# Содержание

1. [Ветки](#Ветки)
2. [Коммиты](#Коммиты)
3. [Сообщения](#Сообщения)
4. [Объединение](#Объединение)


## Ветки

* Выбирайте *короткие* и *описательные* имена:

  ```shell
  # хорошо
  $ git checkout -b oauth-migration

  # плохо - расплывчатое название
  $ git checkout -b login_fix
  ```

* Хорошая практика - использовать номера баг-трекеров, (например, GitHub issue) для именования веток. Например:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* Используйте *дефисы* для разделения слов.

* Когда несколько людей работают над *одной* функцией, может быть удобно создать *командную* и *персональные* ветки. Пример:

  ```shell
  $ git checkout -b feature-a/master # командная ветка
  $ git checkout -b feature-a/maria  # Персональная ветка Марии
  $ git checkout -b feature-a/nikita # Персональная ветка Никиты
  ```
  
  * При создании master ветки для разработки фичи, необходимо создать PullRequest для объединения фичи с основной веткой после завершения разработки, тестирования и сдачи фичи. В Assigners ставим текущего тимлида, или ответственное лицо за техническую реализацию функционала. Это основной PullRequest который можно закрыть только после одобрения Assigner, либо самим Assigner. В описании Master PullRequest описываем реализуемый функционал и основные этапы реализации, условия, после которых ПР можно закрывать. Добавляем к Master PR лейбл команды + "master-feature".
  * Разарбатывая свою часть функционала фичи, например фронт-енд реализацию в ветке `feature-a/front-end`, вы создаете отдельный PullRequest в Master ветку фичи, в этом ПР, помимо привычного описания внесенных изменений, необходимо сделать ссылку на основной Master PR.
  * Таким образом все персональные ПР будут отображаться в основном Master ПР, как этапы.
  * Такие правила позволяют не потерять ветки в которых ведутся изолированные разработки отдельных функциональных частей.
  * Примеры можно посмотреть [здесь](https://github.com/advanta/Product/pull/1511) и [здесь](https://github.com/advanta/Product/pull/1510)

  При желании можно объеденить персональную ветку с командной (см. ["Объединение"](#Объединение))

* Удалите ветку после объединения, если не будете её использовать.

## Коммиты

* Каждый коммит - одно *логическое изменение*. Не делайте несколько *логических изменений* в одном коммите. Например, если вы исправили баг и улучшили производительность, то лучше сделать два разных коммита.

* Не разделяйте одно *логическое изменение* на несколько коммитов. Например, реализация функции и тесты для неё должны быть в одном коммите.

* Делайте коммиты *маленькими*. Маленькие, автономные коммиты легче понять и вернуться, если что-то пойдёт не так.

* Коммиты должны быть *логически* раположены. Например, если *коммит X* зависит от изменений в *коммите Y*, тогда *коммит Y* нужно сделать до *коммита X*.

Примечание: хорошая практика, работая в локальном репозитории, делать коммиты промежуточной работы. Несмотря на это, перед публикацией в сети, стотит выполнить все рекомендации, написанные в этом руководстве.

## Сообщения

* Используйте редактор, а не командную строку, для описания коммита:

  ```shell
  # хорошо
  $ git commit

  # плохо
  $ git commit -m "Quick fix"
  ```

  Использование терминала ограничевает сообщения одной строкой, делая их не информативными.

* Заголовок (первая строка сообщения) должна *кратко*, но *ёмко* описывать изменения. Идеально - не больше 50 символов. Пишите заголовок в повелительном наклонении.

  ```shell
  # хорошо - повелительное наклонение, меньше 50 символов
  Mark huge records as obsolete when clearing hinting faults

  # плохо
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* Потом надо поставить пустую строку, после которой следует описание. Описание состоит из нескольких строк, максимальная длина, которых - 72 символа. Описание должно объяснять почему требуется изменение, как оно решает проблему и какие могут возникнуть ошибки.

  Также нужно оставить ссылки на используемые ресурсы (например, ссылку на соответствующую проблему в трекере ошибок):

  ```text
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Проще говоря, делая коммит, вы должны подумать: "Какая информация понадобиться вам?", если вы наткнётесь на этот коммит через год.

* Если *коммит A* зависит от *коммита B*, укажите эту зависимость в *коммите A*. Используйте хэши SHA1, чтобы ссылаться на коммиты.

  Так же, если *коммит A* решает проблему созданную *коммитом B*, это стоит указать в  *коммите A*.

* Если коммиты будут склеены, то используйте флаги `--squash` и `--fixup` чтобы явно показать свои действия.

  ```shell
  $ git commit --squash f387cab2
  ```

## Объединение

* **Не переписывайте опубликованную историю.** История репозитория ценна сама по себе, и очень важно иметь возможность посмотреть всю историю. Если вы опубликуете изменённую историю, то это вызовет проблемы у тех кто уже работает с репозиторием.

* Однако есть случаи, когда перезапись истории возможна. Например:

  * Вы - единственный, кто пользуется веткой и просматривает её историю.

  * Вы хотите привести в порядок вашу ветку (например, склеить коммиты) и/или перебазировать её на "master", чтобы объединить их.

  Тем не менее, *никогда не переписывайте историю «master» ветки* или каких-либо других специализированых ветвей (например, CI).

* Сделайте историю *чистой* и *простой*. Перед тем, как объединить ветку:

    1. Удостоверьтесь, что она соответствует руководству по стилю, если это не так (измените порядок коммитов, перепишите сообщения и т.д.)

* Если ваша ветка включает в себя более одного коммита, не объединяйте её в режиме fast-forward:

  ```shell
  # хорошо
  $ git merge --no-ff my-branch

  # плохо
  $ git merge my-branch
  ```
