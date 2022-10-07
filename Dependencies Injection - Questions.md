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
**Provider** - это обект вида {token<Type Token, string token & Injection Token>: name, recipe(useClass, useExisting, UseFactory, useValue: ....)}. For example:
~~~
providers: [
  { provide: UseClass(это всего лишь уникальный ключ, наздвание. Может быть любым),
    useClass: UseClass (это название класса который мы инжектируем)
  }
]
~~~
- #### useClass:
> Если token name совпадает с именем класса который используетяс можно сделать короткую запись (shortcut). For example:
```
     ....
     providers: [ UseService ];
```
> useClass создает экземплар класса, ктороый мы указываем в поле recipe. 

- #### useExisting:
> При содании dependecy будет ссылаться на сущ. класс
- #### useValue:
> ссылаетсся на объект а не на класс, как обычно. Когда нужно внести какие то данные кторые должны иметь зависимость. Обычно это какие либо данные или cofig.
- #### useFactory:

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
