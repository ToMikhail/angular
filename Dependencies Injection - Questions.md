- ## Question: Для чего нужен Dependecy Injections (DI)?

  -  *таким образом достигается гибкая архитектура, не допускается дублирование кода.
Позволяет создавать объект, использующий другие объекты.*

- ## Question: Что такое зависимости (dependencies)?
 
  -  *Это объект вида 'ключ-значение'. {provider: injectionToken(name(uniq)), recipe: объект экземпляра класса (object instance class)}*

- ## Question: ?

  -  **

- ## Question: ?

  -  **

- ## Question: ?

  -  **

- ## NOTES:
  - *DI состоит из:*
    - ### *Provider => это обект вида {token<Type Token, string token & Injection Token>: name, recipe(useClass, useExisting, UseFactory, useValue: ....)}*
Если token name совпадает с именем класса который используетяс можно сделать короткую запись (shortcut). For example:
<code><pre>....
     providers: [ UseService ]</pre>
  </code>
    - *Injector (ElemenInjector and ModulInjector);*
     - *Dependency => обычно это экземпляр класса кторый мы создаем* 
