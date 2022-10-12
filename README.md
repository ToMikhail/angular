# angular
- ## 1. Basics and Syntax. Components in Details.
<details>
<summary>more info</summary>

- [ ] [Data-Bindings (interpolation, one-way, two-way)](https://angular.io/guide/binding-syntax).
- [ ] [Component Interaction](https://angular.io/guide/component-interaction).
- [ ] [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks) (https://angdev.ru/doc/component-lifecycle/).
- [ ] [Change Detection Strategy](https://blogs.halodoc.io/understanding-angular-change-detection-strategy/).
- [ ] [ng-content](https://angular.io/guide/content-projection).

</details>
  
  
- ## 2. Directives. Pipes.
<details><summary>more info</summary>

- [ ] [Directive Types (attribute, structural) ](https://angular.io/guide/attribute-directives).
- [ ] [ng-container](https://angular.io/api/core/ng-container), [ng-template](https://angular.io/api/core/ng-template).
- [ ] [Pure & Impure Pipes](https://medium.com/@ghoul.ahmed5/pure-vs-impure-pipe-in-angular-2152cf073e4d).
- [ ] [Async Pipe](https://angular.io/api/common/AsyncPipe).
- [ ] [Custom Directives & Pipes]().

 </details>
 
 
 - ## 3. Services. Dependency Injection Pattern.
<details><summary>more info</summary>

- [ ] Dependency Injection Pattern:
  - [tutorial](https://www.tektutorialshub.com/angular-tutorial/#services-dependency-injection)
  - [video. Сервисы, внедрение зависимостей – Разбор механизма внедрения зависимостей](https://www.youtube.com/watch?v=fALKYP8voBQ),
  - [video. Angular Dependency Injection – Understanding hierarchical injectors](https://www.youtube.com/watch?v=G8zXugcYd7o),
  - [video. Angular dependency injection in depth – Dependency providers (2021)](https://www.youtube.com/watch?v=T1xmCC4y3xo)).
- [ ] [NOTES](https://github.com/ToMikhail/angular/blob/main/Dependencies%20Injection%20-%20Questions.md)

 </details>
 
 
  - ## 4. Forms and Validation.
<details><summary>more info</summary>
  
  Существует 2 подхода реализации форм:  
  
1. Шаблонный подход (template driven forms);
1. Реактивный подход (Reactive forms). Через controls - это экземпляры класса FornControl, FormGroup, FormArray, FormBuilder;

Различия между template-driven forms и Reactive forms:  
- *Reactive forms* - sync, *template-driven forms* - async; 
- *Reactive forms* - управление происходит в класса модели (component). Богатая API. *template-driven form* - управление происходит в шаблоне (в html)(фактически 2-way binding ( [(ngModel)]='var'));
- *Reactive forms*- для сложных форм. *template-driven form* - для протсоых форм;
- *Reactive forms* - гибкость настроек;

Валидатор (validator) - это функция которая возращает функцию (ValidatorFn) которая получает control и синхронно возвращает карту ошибок проверки, если они есть, в противном случае — null.
  
Валидаторы (validator) для форм бывают:
  
  * встроенные (built-in) - required, email, pattern и minLength;
  * пользовательский (custom validators);  
  и
  * async;
  * sync;
  
- [ ] [NOTES](https://github.com/ToMikhail/angular/blob/main/forms.md)

 </details>
 
 
   - ## 5. # Working with Server
   - 
<details><summary>more info</summary>
***HttpClient***
  
  Общение" с сервером Angular осуществляет через REST-подобные запросы. За это отвечает HttpClientModule.  
  В компонент или сервис (в зависимости от построения архитектуры) импортируется сервис HttpClient.  
  For example:
  ```
  @Injectable()
export class DataService {
  constructor(private http: HttpClient) {}
}
  ```
  
***Методы HTTP запросов***
  
  В архитектуре REST API используются разные методы HTTP запросов. Основные:
  - GET - используется для получения данных с сервера
  - POST - используется для создания новой записи
  - PUT - используется для рьновдления существуюшей записис
  - DELETE -  используется для удаления записи.
  
  >Все методы сервиса HttpClient возвращают тип ***Observable*** это означает, что если при вызове метода, который должен сделать HTTP-запрос, не вызвать метод subscribe(), то ничего не произойдет. Методу subscribe() можно передавать две функции-обработчика, первая выполнится в случае успешного ответа от сервера, вторая - в случае ошибки.
  
***Обработка ошибок***
  При работе с HttpClient есть можество путей обработать ошибку.
  1 способ. У метода subscribe(callback1 - удачный ответ с сервера(response); *callback2 - возращает ошибку, где мы ее можем обработать:* callback3 - когда stream  закончился). =>
  ```
  http.get('link').subscribe(res => {}, error => {})
  ```
  2 способ. Передать в методе pipe(catchError(err => {return throwError(error)}))
  ```
  return http('link').pipe(catchError(err => { return throwError(error) }))
  ```

***Http Interceptor***

Angular HTTP Interceptor позволяет перехватывать HTTP-запросы перед их отправкой и вносить в них необходимые изменения. То же самое справедливо и для ответов сервера.

Наиболее частое их применение — отправка авторизационных данных, логирование и обработка серверных ошибок.

Создание сервиса начинается с внедрения интерфейса Angular HttpIntrceptor из @angular/common/http и реализации его метода intercept().

intercept() модифицирует исходный запрос и возвращает объект Observable события HttpEvent<any>, который в свою очередь возвращает метод next() объекта типа HttpRequest.

  
- [ ] [NOTES](https://github.com/ToMikhail/angular/blob/main/working-with-server.md)

 </details>
