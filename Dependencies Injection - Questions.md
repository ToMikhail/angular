- ## NOTES:
Что бы иметь возможность инжектировать в модуль, компонент и др. необходимо добавить декоратор @Injectable(). @Component @Directive имеют под копотом этот декоратор. Если не указать @Injectable() то компонент, модуль не сможет получать в конструкторе DI как параметры. Если в декторатор @Injectable() указано свойотсво provideIn: 'root', таким образом мы регестрируем сервис в app.module.ts в providers. For example:
```
@Injectable({
  provideIn: 'root'; 
})
```
Лучше указывать Dependecies через декортатор @Injectable() в самом файле компонента, чем прописывать это модуле, т.к. Angular производит процедуру tree-shake под капотом и если dependency не используется то она н епойдет в сборку проекта.  
Если service прописан в drivers  через 'root', то он является singlton.  
Example DI:
```
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
  providers: [LocalCounterService]
})

export class AppComponent {

  constructor(public localCounterService: LocalCounterService) {}
  
}
```

  - *DI состоит из:*
1.  Provider;
2.  Injector (ElemenInjector and ModulInjector);
3.  Dependency => обычно это экземпляр класса кторый мы создаем.

### 1. Provider
![](https://www.tektutorialshub.com/wp-content/uploads/2017/01/Angular-Provider.png);  
**Provider** - это обект вида {token<Type Token, string token & Injection Token>: name, recipe(useClass, useExisting, UseFactory, useValue: ....)}. For example:
~~~
providers: [
  { provide: UseClass(это всего лишь уникальный ключ, наздвание. Может быть любым),
    useClass: UseClass (это название класса который мы инжектируем)
  }
]
~~~

Angular Dependency Injection предоставляет 4 типа провайдеров:
1. Class Provider : useClass
1. Value Provider: useValue
1. Factory Provider: useFactory
1. Aliased Class Provider: useExisting.

- #### useClass:
> Если token name совпадает с именем класса который используетяс можно сделать короткую запись (shortcut). For example:
```
     ....
     providers: [ UseService ];
```
> useClass создает экземплар класса, ктороый мы указываем в поле recipe. 

- #### useExisting:
> При содании dependecy будет ссылаться на экземпляр уже сущ. класса.

- #### useValue:
> ссылается на объект данныч а не на класс (value, array, object), как обычно. Когда нужно внести какие то данные кторые должны иметь зависимость. Обычно это какие либо данные или cofig (URL, API). В конструктор класса тогда добавляется декортатор @Inject(). For example:  
```
....
  providers: [
    {
      provide: 'FUNC',
      useValue: () => {
        return 'hello';
      }
    }
  ]
}

export class AppComponent {
  constructor(
    @Inject('FUNC') public someFunc: any
  ) {
    console.log(someFunc());
  }
}
```
- #### useFactory:
> Реализует функцию по которой вернется экземпляр service класса  или значение. Обычно мы используем useFactory, когда хотим вернуть объект на основе определенного условия. При использовании useFactory вводится 3 свойство в провайдер зависимости - deps( это массив использванных в функции параметорв(классов)).  
> For example:
```
  providers: [
    { provide: LoggerService, useClass: LoggerService },
    { provide: 'USE_FAKE', useValue: true },
    {
      provide: ProductService,
      useFactory: (USE_FAKE, LoggerService) =>
        USE_FAKE ? new FakeProductService() : new ProductService(LoggerService),
      deps: ['USE_FAKE', LoggerService]
    }
  ]
```


### 2. Injector
> **Injector** - это обстракция через кторую строится взаимодействие между потребителями (consumer) и поставщтками (provider) зависимости (dependency) может быть 2 типов (ElemenInjector and ModulInjector).
> - #### ElemenInjector
>ElemenInjector  - создается для каждого элемента кторый имеет зависимостию
> - #### ModulInjector
> ModulInjector - создается на уровне модуля.
>
> Injector ищет dependency по token в providers по цепочке (вверх по дереву), начииная с элемента. Если не находит в ElementInjectors идет в ModuleInjector и затем еще выше в NullInjector. Если Dependency не обнаружена NullInjector выдает ошибку (Error);

### 3. Dependency;
> Dependency обычно это экземпляр класса кторый мы создаем. Dependencies хронятся в массиве providers;


- ## Question: Для чего нужен Dependecy Injections (DI)?

  -  *таким образом достигается гибкая архитектура, не допускается дублирование кода.
Позволяет создавать объект, использующий другие объекты.*

- ## Question: Что такое зависимости (dependencies)?
 
  -  *Это объект вида 'ключ-значение'. {provider: injectionToken(name(uniq)), recipe: объект экземпляра класса (object instance class)}*

- ## Question: ?

  -  **;
