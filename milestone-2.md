**Версии проекта**
Добавить в модель проекта строку с версией. Кажется наиболее простой вариант для изменения существующей структуры, который позволит отслеживать историю изменения ФТ и выполнять тестовые прогоны на разных версиях. Параметр версии опциональный, повторный экспорт с существующим в БД номером версии работает как и текущем варианте обновляя существующее содержимое. Экспорт с унимальным номером версии работает аналогично созданию нового проекта.
**Линтер**
**TestRun-API**

Ниже подробнее об изменениях
# specbox-api
## RESTAPI
### GET projects/list #deprecate
Список проектов. /list лишнее уточнение для получения списка проектов
### GET projects #new
Список проектов
### GET projects/{project}/versions #new
Список версий
### POST projects/{project}/versions/{version} #new
Экспорт проекта (предложение сделать экспорт в этом маршруте)
### GET projects/{project}/features/{feature} #deprecate
Возвращает фичу
### GET projects/{project}/versions/{version}/features/{feature} #new
Возвращает фичу проекта в указанной версии
### GET projects/{project}/structure #deprecate
Возвращает первое дерево или структуру по-умолчанию
### GET projects/{project}/versions/{version}/structures #new
Список деревьев
### GET projects/{project}/versions/{version}/structures/{structure} #new
Получение дерева

### GET export/upload?project&version #new
Экспорт проекта
Может следует перенести ручку в POST projects/{project}/versions/{version} ?

### POST stat/upload-autotests?project&version
Загрузка результатов автотестов - такая ручка будет работать только если не менялись сами фичи
### GET stat?project&version&from&to
Получение статистики

## Обработка ошибок
Добавить обработчики ошибок для ручек

## Domain

Добавить `version` для проекта
Добавить модель описания метрик
Добавить модель связи метрики с assertion

# specbox-sync

## Версионирование проектов
Поддержка опционального аргумента version при синхронизации

## Атрибуты для assertion
В yaml для assertion добавить атрибуты - требуется для отделения утверждений, которые еще не реализованы, от готовых в проекте. Нарушает ли это идеологию? Или мы должны действовать от того что в yaml описано только то что должно УЖЕ быть реализовано в коде.

## Метрики для assertion
В мете описать метрики
```yaml
metrics:
  - code: size
    title: Размер (кБ)
    type: number
  - code: duration
    title: Длительность (с)
    type: number
```
В assertion добавить метрики
```yaml
feature: Нефнкциональные требования
code: nonfunc

specs-unit:
  Размер бандла:
    - assert: Размер первично загружаемого модуля не превышает 1 500 кБ
      metrics:
        - size
  Скорость ручек:
    - assert: Загрузка дерева фичей не превышает 5 секунд при канале 5 МБит/с
      metrics:
        - duration
```

## Рефакторинг для линтинга
Выделение инструментов сбора yaml и валидации в программный API для использования в линтере

# specbox-eslint
Реализовать правила валидации, использовать API из sync

# specbox-testrun-api
## Domain
### TestRun 
Модель тестового прогона
```C#
public class TestRun
{
  public Guid Id { get; set; }

  public string Title { get; set; } = null!;
  public string? Description { get; set; }
  public DateTime CreatedAt { get; set; }
  public DateTime? StartedAt { get; set; }
  public DateTime? CompletedAt { get; set; }

  public Guid ProjectId { get; set; }
  public Project Project { get; set; } = null!;
}
```
### TestResult 
Модель результата теста
```C#
public class TestResult
{
  public Guid Id { get; set; }

  public string Status { get; set; } = null!;
  public string? Report { get; set; }
  public DateTime? CompletedAt { get; set; }
  
  public Guid AssertionId { get; set; }
  public Assertion Assertion { get; set; } = null!;
  public Guid TestRunId { get; set; }
  public TestRun TestRun { get; set; } = null!;
}
```
### TestResultMetric
Модель результата в виде метрики
```C#
public class TestResultMetric
{
  public Guid Id { get; set; }

  public Guid TestResultId { get; set; }
  public TestResult { get; set; }
  public Guid MetricId { get; set; }
  public ?????? { get; set; }

  public string? Value { get; set; }
  public DateTime? CompletedAt { get; set; }
}
```
## REST API
### POST projects/{project}/versions/{version}/testruns
Создать тестовый прогон
Модель body
```C#
public class CreateTestRun
{
  [Required] public string Title { get; set; } = null!;
  public string? Description { get; set; }
}
```
### GET projects/{project}/versions/{version}/testruns
Список тестовых прогонов
Модель результата - массив TestRunModel
```C#
public class TestRunModel
{
  public Guid Id { get; set; }
  [Required] public string Title { get; set; } = null!;
  public string? Description { get; set; }
  [Required] public string ProjectCode { get; set; } = null!;
  [Required] public DateTime CreatedAt {get;set;}
  public DateTime? StartedAt {get;set;}
  public DateTime? CompletedAt {get;set;}
}
```
### GET projects/{project}/versions/{version}/testruns/{testRunId}/testresults
Список результатов тестирования
```C#
public class TestResultModel
{
  [Required] public Guid Id { get; set; }

  [Required] public string Status { get; set; } = null!;
  public string? Report { get; set; }
  public DateTime? CompletedAt { get; set; }

  public string AssertionTitle { get; set; } = null!;
  public string AssertionGroupTitle { get; set; } = null!;
  public string FeatureCode { get; set; } = null!;
  public string FeatureTitle { get; set; } = null!;

  public TestResultMetricModel[] Metrics { get; set; }
}

public class TestResultMetricModel
{
  [Required] public Guid Id { get; set; }

  public string? Value { get; set; }
  public DateTime? CompletedAt { get; set; }

  public string Code { get; set; } = null!;
  public string Title { get; set; } = null!;
}
```
