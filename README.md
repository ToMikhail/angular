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
 
 
   - ## 5. Working with Server

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
 
  - ## 6. Routing

<details><summary>more info</summary>
  
- Порядок создания routing:
1. Создаем компоненты страницы приложения (страницы или routes);
2. создаем routing модуль, который содержит @NgModule декоратор со свойствами ***imports*** и ***exports***. И в imports передаем пути для строниц (routes).
  ***routes*** - это массив объектов [{},{},{}] со свойствами path: 'путь до строницы', component: имя класса отвечающего за компонент, (это обязательные), и по требованию свойство children: с массивом обхектов (дочерних страниц).   
  Вместо component может идти свойство  ***redirectTo*** c указанием страницы (route) куда необходимо перенаправить в случае неверног гзадание ссылки (ошибка в пути).
>  
```
const routes: Routes = [
  {path: '', component: HomeComponent},
  {path: 'about', component: AboutComponent, children: [
    {path: 'extra', component: AboutExtraComponent}
  ]},
  {path: 'posts', component: PostsComponent},
  {path: 'posts/:id', component: PostComponent},
  {path: '**', redirectTo: ''},
]

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export default class AppRoutingModule {}
```
  
  3. Регестрируем созданный модуль с app.module.ts в imports [];
  4. В app.component.html => прописывается тег **router-outlet**  
  
- Директива ***routerLink***   
  Используется вместо href у ссылок. Может навешиваться на любой тег. Осуществляет ререндеринг страницы. 
  
  ```
  [routerLink]="['/posts']"
  ```
  Навигация может быть внутри шаблона через ***routerLink*** и програмной, внутри component.ts (програмное взаимодействие), через (click) в шаблоне.    
  Чтобы работать в .ts с routering необходмо в component в  constructor передать экземпляр класса Router (Можно менять страница через метод router.navigate(['путь'])).   
  Чтобы работать со ссылками необходимо в component в  constructor передать экземпляр класса ActiveRoute(доступ до querryParams, fragment) 
  
- ***Guards (Route Guards)***  - позволяют ограничить доступ к маршрутам на основе определенного условия, например, только авторизованные пользователи с определенным набором прав могут просматривать страницу.
  
  Различают следующие виды guard-ов:
    * CanMatch - разрешает видеть или не видеть  маршрут (route)  - 1-й проверяется в "жизненном цикле" (новый)
    * CanActivate - разрешает/запрещает доступ к маршруту - 4-й проверятеся;;
    * CanActivateChild -разрешает/запрещает доступ к дочернему маршруту - 5-й проверятеся;;
    * CanDeactivate - разрешает/запрещает уход с текущего маршрута - 3-й проверятеся;
    * Resolve - выполняет какое-либо действие перед переходом на маршрут, обычно ожидает данные от сервера - 6-й проверятеся;;
    * CanLoad - разрешает/запрещает загрузку модуля, загружаемого асинхронно - 2-й проверятеся;.
  
  Можно задать много guards для route.
  
- [ ] [NOTES](https://github.com/ToMikhail/angular/blob/main/routing.md)

</details>
  
  
  - ## 7. Modules
  
<details><summary>more info</summary>
  
  Angular имеет модеульную архитектуру, в отличии от React(компонентную). Масшиабиаемость 
  
  Для создания отдетного модуля создаем файл .module.ts в ктором @NgModule  - декортатор и класс.
  
```
@NgModule({
  declarations: [
    AppComponent,
    AboutComponent,
    HomeComponent,
    PostsComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
  
  - declorations: - это массив где решистрируются все компоненты, директивы и пайрыж
  - imports: - это массив, куда импартируются другие модули(BrowserModule? FormsModule, свои созданные модулиж
  - providers - это массив , где регистриуются сервисыж
  - bootstrap - для главного компонента приложения (точка входа).
  
  Для общих сущностей которые используются в разных модулях создается отдельный модуль(обычно в папке shared). Сущности заносятся в declorations (массив) и возвращаются в exports (массив). Затем в imports (массиве) зарегестрировать shared модуль app. и др.
  
  Метод forRoots(routes) - только для app.module;
  Метод forChild(routes) - используется для остальных модулей приложения при маршрутизации.
  
  Для оптимизации и декомпозирования элементов.
  
  Lasy loading - отложенная загрузка.
  
  При работе с разбивкой на модули есть возможность разбивики на части (на chunks). Позволит загружать сначала необходимый для рендеринга компоненет, а при переходе будет загружать тоже необходимый компонет(chunk)
  
  В  app-routing.module.ts доббваить объект с path: '...' и loadChildren: 'link#classModule(класс модуля)'. Новый синтаксиси позволяет передовать функцию (стрелочную):
  
  ```
  loadChildren: () => import('link').then(m => m.AboutPageModule)
  ```
  
  ***Изменить стратегию загрузки*** в app.modile.ts необходимо в app.module.ts
  
  ```
  imports: [
    RouterModule.forRout(routes),
    {preLoadingStrategy:PreloadingAllModules (или noPreloading)}
  ]
  ```
  
    Это позволяет изменить загрузку компонентов. В первую очередьзагрузятся все необходимые компоеннеты для рендеринга, а после все остальные (для роутинга). Что бы можно было перехолдить через ссылки без перезагрузок.
  
  
 </details>

  
  - ## 8. RxJS Basics
  
<details><summary>more info</summary>
  
  RxJS представляет собой библиотеку, позволяющую управлять всеми асинхронными операциями и событиями в приложении в стиле реактивного программирования. Мы можем подписаться на stream и отлавливать све что происходит с этим стриомом (все изменения)
  
  * Observable;
  > Объекты RxJS Observable создаются либо с использованием операторов создания (of, from, fromEvent), либо через конструктор new Observable.

Пример с оператором of().

```
of('Hello').subscribe((vl) => console.log(vl));
  ```
Пример с new Observable.

```
const obs = new Observable((sub) => {
  sub.next(1);

  setTimeout(() => {
    sub.next(3);
    sub.complete();
  }, 500);
});
  
obs.subscribe((vl) => console.log(vl));
  ```
  
  * Observer;
  >Observer — это объект у которго есть 3 метода (next, error и complete). - это потребитель (subscruber) значений, предоставляемых Observable. Observer — это просто набор обратных вызовов (callbacks funcs), по одному для каждого типа уведомления, доставляемого Observable: next, error и complete. Ниже приведен пример типичного объекта Observer:
  ```
      const observer = {
      next: (val:any) => console.log(val),
      error: (err: any) => console.error('error ', err),
      complete: () => console.log('done')
    }

    obs.subscribe(observer);
  ```
  
  * Subject
  >Subject является разновидностью объектов Observable. Особенность Subject в том, что он может отправлять данные одновременно множеству "потребителей" (multicasetd), которые могут регистрироваться уже в процессе исполнения Subject, в то время как исполнение стандартного Observable осуществляется уникально (uniquecasted) для каждого его вызова.   
  >Объекты RxJS Subject реализуют принцип работы событий, поддерживая возможность регистрировать неограниченное количество обработчиков отправляемых ими данных.
  
  ```
  const sbj = new Subject<number>();

sbj.subscribe((vl) => console.log(`1st: ${vl}`));
sbj.next(3);
sbj.subscribe((vl) => console.log(`2nd: ${vl}`));
sbj.next(9);

/*
Результат  в консоли:

1st: 3
1st: 9
2nd: 9
*/
```
  
  В RxJS имеется несколько разновидностей Subject:
    - BehaviorSubject - хранит в себе последнее отправленное им значение,каждому новому обработчику в момент регистрации (вызов subscribe()) будет отправлено это значение;
    - ReplaySubject - способны хранить заданное количество последних значений, которое задается при создании объекта( new ReplaySubject(2) );
    - AsyncSubject - "потребителям" передается только последнее значение объекта и только, когда он завершит свое выполнение (вызов complete()).
  
  * Scheduler - пока не надо;
  * Subscription   
  Subscription- это объект, который представляет одноразовый ресурс, обычно выполнение Observable. У подписки есть один важный метод, отмена подписки (unsibscribe()), который не принимает аргументов и просто удаляет ресурс, удерживаемый подпиской.
  Subscription, по сути, просто имеет функцию unsubscribe() для освобождения ресурсов или отмены выполнения Observable.
  
  * Operator.     
  
  Оперторов бывает очень много: 
  Операторы высшего порядка(mergeMap, SwichMap, ExhaustMap, concatMap). Вложенные стримы
    - mergeMap(flatMap()) - Стрим содержит другие стримы, все стримы важны, если 1-й стрим не закончился, а 2-й начался, то пойдут данные из 2-го стрима, а потом вернуться к 1-му, и закончат его и перейдут ко второмуж
    - SwichMap - если при выполнении 1-го стрима начинается 2-й стрим, то главный стрим убивает 1-й стрим и переходит к выполнению 2-го и т.д.
    - ExhaustMap - главный прим начал принмать данные 1-го стрима, когда начинают потсопуть данные из 2-го стрима, главный стрим их игнорирует до момента окончания 1-го стрима, и переходит к следующему приходящему стриму (если подойдет 3-й стрим, то будет обрабатываться он, 4-й - значит четвертый);
    - concatMap() - обслуживает все стримы по очередно, не важно когда через какие интервалы приходили данные от стримов.   
  
  Операторы объединения - позволяют объединять информацию из нескольких observable. Порядок, время и структура выдаваемых значений являются основными различиями между этими операторами:
    - forkJoin - получаем объект. Когда нескольок subscriber и необходимо получить только конечные значения каждого из них. Анналог Promise.All;
    - zip - выдает массив значений после получения поаледнего;
    - combineLatest() - выдает массив последних значений стримов, при получении одного из них.   
  Операторы создания - Эти операторы позволяют создавать observable практически из чего угодно. От общих до конкретных вариантов использования вы можете и поощряете превращать все в поток.   
    - of() - выдает стрим переданных в него значений (строки, числа, массив и т.д.);
    - from() - 1) для конвертации Promise в Observable. 2) для получения значений элементов переданного массива. 3) передванного объекта. Любых перебираемых итерируемых объектов;
    - fromEvent() - Превратить событие в наблюдаемую последовательность ( const source = fromEvent(document, 'click'); );
    - interval() - Выдавать числа последовательно в зависимости от предоставленного таймфрейма ( const source = interval(1000); )
  
  [Hot observable vs Cold Observable](https://www.youtube.com/watch?v=oKqcL-iMITY&t=14s&ab_channel=DecodedFrontend)  
  ***Cold Observable (lazy) (unicated)*** - начинают передовать данные только когда мы подпишимся(выполним метод subscribe()) на них. Каждый subscribe() создает отдельный контекст выполнения Observable (пример с получением Math.random - каждый subscribe() вернет разное значение). Создает и активирует данные в Observable;  
  ***Hot observeable (multycasted)*** - получают данные всегда, независмо сделали мы подписку или нет (subscribe()). Создает и активирует данные вне Observable;   
  ***Warm observeable (подогретый)*** - cold observable можно подогреть. Используя оператор multicasted() куда мы передвем Subject(). Затем объединяем два стрима с помощью метожа connect();   
  
  Из Promise в Observable можно перевести через оператор from()
  
  Разница между Promise и  observable   
  Promise выполнили один раз и уничтожили данные, Observable - это стрим , кторый можно использовать много раз.
  ---
  Как отписаться от стрима?   
  1. Отписаться от стрима через метод unsubscribe()[https://blog.bitsrc.io/6-ways-to-unsubscribe-from-observables-in-angular-ab912819a78f];
  1. Опрератор 
    * take(num) - где num - это количество получаемых входных данных;
    * takeUntil(notifier);
    * takeWhile(val => val < 5);
    * first() - если без аргументов то выдаст первое приходящее значение. если first(val => val === 5) - dslfcn только 5
 </details>
