> - [Русский](https://github.com/bakhirev/assayo/blob/main/documents/RU.md)
> - [English](https://github.com/bakhirev/assayo)

# [Assayo](https://assayo.jp/?ref=github&lang=ru)

Visualización y análisis de los datos de su repositorio git ([demostración](https://assayo.jp/demo/?dump=./test.txt).

##### Сотрудник может оценить новое место работы
- темп работы;
- количество переработок;
- зоны ответственности;
- объем фичей и багов;
- стиль работы коллег;

##### Руководитель может оценить сотрудников
- выявить бездельников;
- прикинуть объём кода;
- узнать скорость работы;
- заметить аномалии поведения;
- посмотреть динамику работы по неделям;

##### Инвестор может оценить продукт
- стоимость продукта;
- стоимость фичей;
- время на разработку;
- прогноз времени доработок;
- прогноз стоимости;

### Как быстро посмотреть количество коммитов?

В корневой директории вашего проекта выполнить:
```
git shortlog -s -n -e
```
### Как объединить авторов?
В корневой директории вашего проекта нужно создать файл `.mailmap`.
Пример содержания файла:
```
Alex B <alex@mail.uk>
Alex B <alex@mail.uk> <alex@gov.tk>
Alex B <alex@mail.uk> <bakhirev@ya.kz>
Alex B <alex@mail.uk> <man64@yahoo.com>
``` 
Подробнее про формат этого файла можно прочитать [тут](https://git-scm.com/docs/gitmailmap).

### Как выгрузить данные из git?

#### Для онлайн просмотра
В корневой директории вашего проекта выполнить:
```
git --no-pager log --numstat --oneline --all --reverse --date=iso-strict --pretty=format:"%ad>%cN>%cE>%s" > log.txt
```
#### Для офлайн просмотра

```
git --no-pager log --numstat --oneline --all --reverse --date=iso-strict --pretty=format:"%ad>%cN>%cE>%s" | sed -e 's/\\/\\\\/g' | sed -e 's/`/"/g' | sed -e 's/^/report.push(\`/g' | sed 's/$/\`\);/g' | sed 's/\$/_/g' > log.txt
```
Git создаст файл `log.txt`.
Он содержит данные для построения отчёта. 

Разница между онлайн и офлайн форматом в наличие обёртки для строк. Оффлайн формат будет подтягиваться, как `js` файл если вы просто открыли `/build/index.html`

### Как посмотреть отчёт онлайн? 

- Перейти на [сайт](https://assayo.jp/)
- Нажать кнопку «[Демо](https://assayo.jp/demo)»
- Перетащить файл `log.txt` в окно браузера

### Как посмотреть отчёт офлайн?
- Скачать этот репозиторий
- Перетащить файл `log.txt` в папку `/build`
- Запустить `/build/index.html`
- Или перетащить папку `/build` к себе в репозиторий (туда, где лежит `log.txt`). Можно сменить название. Например с `/build` на `/report`

В этом случае важно, чтобы файл `log.txt` был сгенерирован командой для офлайн просмотра.

### Как пересобрать билд отчёта?
- Скачать этот репозиторий
- Выполнить `npm install`
- Выполнить `npm run build`
- Свежая сборка будет в папке `/build`

### Как посмотреть отчёт по группе микросервисов?
- Сгенерировать для каждого микросервиса файл `log.txt` (`log-1.txt`, `log-2.txt`, `log-3.txt` и т.д.)
- См. «Как посмотреть отчёт онлайн?». На последнем шаге перетащить сразу все файлы в окно браузера.
- См. «Как посмотреть отчёт офлайн?». На втором шаге перетащить все файлы микросервисов (`log-1.txt`, `log-2.txt`, `log-3.txt` и т.д.) в папку отчета (`/build`).

### Как брендировать интерфейс?
Вы можете написать свою тему для интерфейса. Можно менять:
- **Заголовок**. Вы можете указать его в URL-параметре ```title```. Например: ```?title=You Company```
- **Визуальную тему**. Для этого нужно подготовить CSS файл с новыми стилями и указать его адрес в URL-параметре ```theme```. Например: ```?theme=//company.com/some.css```. Вы можете использовать имена классов в качестве селекторов. Большинство из них не меняется в при выходе новой версий.
- **Язык**. Вы можете указать его в URL-параметре ```lang```. Например: ```?lang=es```

### Как подписывать коммиты?

Следуйте практике [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/). Например:
```
JIRA-1234 feat(profile): Added avatar for user 
```
- номер задачи в таск трекере `(JIRA-1234)`
- тип работы `(feat, fix, style, refactor, test, doc и т.д.)`
- фича `(profile - раздел сайта, страница или новый функционал, одним словом)`
- какую проблему решали `(Added avatar for user)`

### Как автоматизировать сбор данных?

#### Без бекенда
- создайте клон нужного вам репозитория;
- скопируйте в корень папку `build`;
- откройте `build/index.html` в браузере и добавьте в закладки;
- добавьте ярлык на `build/assets/ci-cd.sh` в папку автозагрузки (Windows);

Каждый раз, при перезагрузке компьютера, скрипт будет обновлять статистику по всем данным, которые автоматически влились в основную ветку.

### DevOps (CI/CD)

#### Публичный сервер

Вы можете выкладывать файл с данными для построения отчёта на публичный URL. А для его визуализации использовать веб-интерфейс сайта [assayo](https://assayo.jp/). Просто укажите адресс, где лежат данные, в URL-параметре ```dump```:
```
https://assayo.jp/demo/?dump=//you_site.com/some/log.txt
```

#### Приватный сервер
- скачайте [docker образ](https://hub.docker.com/r/bakhirev/assayo);
- поднимите его в локальной сети;
- для просмотра отчётов используйте веб-интерфейс указывая ему адресс, где лежат данные, в URL-параметре ```dump```:
```
http://assayo_url/?dump=//you_url/some/log.txt
assayo_url - URL адресс контейнера assayo, он слушает 80 порт;
you_url    - URL адресс вашего контейнера с логами git;
```

По умолчанию образ запустится по адресу ```http://127.0.0.1:80/```. Если не получилось проверьте свободен ли у вас 80 порт.
#### Обновление Docker-образа

- удилить метрику, аллерты, старые билды;
- собрать билд ```npm run build```
- собрать образ ```docker build -t assayo .```
- визуально проверить образ ```docker run --name assayo -p 80:80 -d assayo```;
- поставить тег ```docker tag IMAGE_ID bakhirev/assayo:latest```;
- запушить образ в Docker Hub

### Релизы, примерно, раз в полгода. Что дальше:

- больше советов и достижений;
- итоги года / месяца, печать отчётов;
- локализация и интернационализация;
- анализ файлов;
- разные роли для статистики (скрытие финансов);
- разработка бекенда, интеграции с другими системами;

### Как добавить или отредактировать перевод?

Вы можете добавить новый перевод или поправить текущий в разделе ```ts/translations/```.
[Инструкция](https://docs.github.com/ru/get-started/exploring-projects-on-github/contributing-to-a-project)

### Пожелания, предложения, замечания
- telegramm [@bakhirev](https://t.me/bakhirev) (приоритетный способ связи)
- [alexey-bakhirev@yandex.ru](mailto:alexey-bakhirev@yandex.ru)
- сайт [https://assayo.jp/](https://assayo.jp/)
