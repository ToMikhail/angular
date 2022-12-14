# Working with Server

  
  ## HttpClient
  
  Общение" с сервером Angular осуществляет через REST-подобные запросы. За это отвечает HttpClientModule.  
  В компонент или сервис (в зависимости от построения архитектуры) импортируется сервис HttpClient.  
  
  For example:
  ```
  
  @Injectable()
export class DataService {
  constructor(private http: HttpClient) {}
}

  ```
  
  ### Методы HTTP запросов
  
  В архитектуре REST API используются разные методы HTTP запросов. Основные - ***GET, POST, PUT и DELETE***.
  
  >Все методы сервиса HttpClient возвращают тип Observable. Это означает, что если при вызове метода, который должен сделать HTTP-запрос, не вызвать метод subscribe(), то ничего не произойдет. Методу subscribe() можно передавать две функции-обработчика, первая выполнится в случае успешного ответа от сервера, вторая - в случае ошибки.

#### GET-запрос
Пример Angular GET-запросов.

```
//GET-запрос на получение списка счетов
getAccounts(){
    return this.http.get('http://example.com/api/accounts');
}

//GET-запрос на получение счета по id, id передается как GET-параметр
getAccountByID(id: number | string){
    return this.http.get('http://example.com/api/accounts', {
        params: new HttpParams().set('id', id)
    });
}
```

Для задания GET-параметров ***get()*** в качестве второго аргумента передается конфигурационный объект со свойством params. Здесь же в объекте указываются и другие параметры, например, headers, в котором задаются заголовки ответа. Все параметры подробно рассмотрены далее.

Свойство params принимает экземпляр класса HttpParams, который предварительно импортируется.

```
import { HttpParams } from '@angular/common/http'
```

Передача значений осуществляется с помощью set(). Для передачи множества параметров используется следующая запись.

```
params: new HttpParams()
  .set(`id`, id)
  .set(`category`, category)
```

#### POST-запрос
Метод ***post()*** принимает три аргумента. Второй - тело запроса, третьим параметром можно передаваться конфигурация.
Пример Angular POST-запроса.

```
//POST-запрос на создание нового счета:
createAccount(){
    return this.http.post(`http://example.com/api/accounts`, {
        name: `Default account name`,
        type: 1,
    });
}
```

#### PUT-запрос
Пример Angular PUT-запроса

```
//PUT-запрос на создание нового счета
updateAccount(){
    return this.http.put(`http://example.com/api/accounts`, {
        name: `Updated account name`,
        id: 3,
    });
}
```
Метод ***put()*** во всем идентичен методу post(). Разница между ними состоит в том, что post() используется для создания новой записи, а put() - для ее обновления.

#### DELETE-запрос

Пример Angular DELETE-запроса.

```
//DELETE-запрос на получение счета по id, id передается как GET-параметр
deleteAccountByID(id: number | string){
    return this.http.delete(`http://example.com/api/accounts`, {
        params: new HttpParams().set(`id`, id)
    });
}
```

***delete()*** используется для удаления записи. В своем использовании он схож с GET. Оба метода не имеют тела запроса, а данные передают в строке запроса.


#### Cвойства в конфигурации

Теперь рассмотрим, какие свойства можно задать в конфигурации:
* ***headers*** - принимает экземпляр класса HttpHeaders, который содержит указанные с помощью метода set(key: string, value: string) HTTP-заголовки; класс HttpHeaders должен быть предварительно импортирован;
* ***params*** - принимает экземпляр класса HttpParams, который содержит указанные с помощью метода set(key: string, value: string) параметры строки запроса;
* ***reportProgress*** - указывает, необходимо ли при загрузке на сервер или скачивании с сервера данных передавать информацию о текущем состоянии; принимает либо true, либо false (по умолчанию null);
* ***responseType*** - указывает тип данных ответа ('arraybuffer' | 'blob' | 'json' | 'text'); по умолчанию 'json';
* ***withCredentials*** - указывает, будут ли в запросе передаваться авторизационные данные пользователя; принимает либо true, либо false (по умолчанию).
Для получения исчерпывающих данных о процессе выполнения запроса в его параметрах необходимо установить reportProgress в true.

```
@Injectable()
export class ContactsService {
  constructor(private http: HttpClient) {}

  getContactsDictionary() {
    const req = new HttpRequest(
      'GET',
      '/api/contacts/dictionary',
      {
        reportProgress: true,
        responseType: 'blob',
      }
    )

    this.http
      .request(req)
      .subscribe((event: HttpEvent<any>) => {
        switch (event.type) {
          case HttpEventType.Sent:
            console.log('Sent')
            break
          case HttpEventType.DownloadProgress:
            console.log(
              `Downloading: ${event.loaded / 1024}Kb`
            )
            break
          case HttpEventType.Response:
            console.log('Finished', event.body)
        }
      })
  }
}
```
#### метод request()
Запросы такого рода реализуются через ***метод request()*** (см. выше), а их состояние отслеживается с помощью объекта HttpEvent. В его свойстве type указывается тип текущего события (HttpTypeEvent).

Классификация HttpTypeEvent:
* Sent - запрос был отправлен;
* ResponseHeader - получены код и заголовки ответа;
* UploadProgress - событие отправки данных (для POST и PUT);
* DownloadProgress - событие скачивания данных (для GET);
* User - событие от HttpInterceptor или сервера;
* Response - выполнение запроса завершено.


### Обработка ошибок

  При работе с HttpClient есть можество путей обработать ошибку.  
  
  1 способ. У метода subscribe(callback1 - удачный ответ с сервера(response); *callback2 - возращает ошибку, где мы ее можем обработать:* callback3 - когда stream  закончился). =>
  ```
  http.get('link').subscribe(res => {}, error => {})
  ```
  2 способ. Передать в методе pipe(catchError(err => {return throwError(error)}))
  ```
  return http('link').pipe(catchError(err => { return throwError(error) }))
  ```


### Http Interceptor

Angular HTTP Interceptor позволяет перехватывать HTTP-запросы перед их отправкой и вносить в них необходимые изменения. То же самое справедливо и для ответов сервера.

Наиболее частое их применение — отправка авторизационных данных, логирование и обработка серверных ошибок.

Начнем сразу с примера и детально его рассмотрим.

```
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor() {}

  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    const authReq = req.clone({
      headers: req.headers.set('Session', '123456789'),
    })

    return next.handle(authReq).pipe(
      tap(
        (event) => {
          if (event instanceof HttpResponse)
            console.log('Server response')
        },
        (err) => {
          if (err instanceof HttpErrorResponse) {
            if (err.status == 401)
              console.log('Unauthorized')
          }
        }
      )
    )
  }
}
```

Создание сервиса начинается с внедрения интерфейса Angular HttpIntrceptor из @angular/common/http и реализации его метода intercept().

intercept() модифицирует исходный запрос и возвращает объект Observable события HttpEvent<any>, который в свою очередь возвращает метод next() объекта типа HttpRequest.

В качестве аргумента next() принимает модифицированный объект запроса.

Для манипуляций с объектом запроса используется метод ***clone()***, вызываемый у экземпляра класса HttpRequest. Он позволяет изменять свойства объекта, пока создает его копию, и возвращает его модифицированный экземпляр.

Ответы сервера также попадают в метод intercept(). Чтобы "перехватить" их, нужно определить тип события (any в HttpEvent<any>).

Сделать это можно используя оператор tap(), который принимает в качестве аргумента возвращаемое значение Observable. Все успешные ответы сервера принадлежат к классу HttpResponse, ошибки — к классу HttpErrorResponse.

В зависимости от ответа сервера объект события содержит соответствующие событию данные.

В приложение Angular HTTP Interceptor внедряется аналогично сервису.

```
providers: [
  AuthService,
  AuthGuard,
  {
    provide: HTTP_INTERCEPTORS,
    useClass: AuthInterceptor,
    multi: true,
  },
]
```
  
Сперва необходимо импортировать injection-токен HTTP_INTERCEPTORS, затем в массиве providers записать объект, подобный тому, что записан в примере выше.

Обязательный параметр {multi: true} говорит, что injection-токен HTTP_INTERCEPTORS внедряет не одно, а массив значений. Такой механизм позволяет создавать в приложении Angular неограниченное количество HTTP Interceptor-ов.
