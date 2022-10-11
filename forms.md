# Forms and Validation.

  Существует 2 подхода реализации форм:  
  
1. Шаблонный подход (template-driven forms). 2-way binding ( [(ngModel)]='var');
1. Реактивный подход (Reactive forms). Через controls - это экземпляры класса FornControl, FormGroup, FormArray, FormBuilder;

## Различия между template-driven forms и Reactive forms:  
- *Reactive forms* - sync, *template-driven forms* - async; 
- *Reactive forms* - управление происходит в класса модели (component). Богатая API. *template-driven form* - управление происходит в шаблоне (в html)(фактически 2-way binding ( [(ngModel)]='var'));
- *Reactive forms*- для сложных форм. *template-driven form* - для протсоых форм;
- *Reactive forms* - гибкость настроек;


## Шаблонный подход (template-driven forms).
Фактически, это когда привязка между формой и моделью (классом) происходит через 2-way binding ( [(ngModel)]='var').  Управление формой происходит в класса модели (component).

## Реактивный подход (Reactive forms)
Чаще всего используют *reactive forms*. В кллассе через метод ogOnInit() - во время инициализации компонента. Через добавление экземпляры класса FormGroup, FormControl, FormArray.   
В constructor FormControl передается:  
1. аргумент - первоначальное значение control;
2. аргумент - передаются валидаторы (validators). Либо функция с валидацией либо массив функцийю

For example:

```
  ngOnInit(): void {
    this.form = new FormGroup({
      email: new FormControl('', [
        Validators.email,
        Validators.required,
      ]),
      
      password: new FormControl('', [Validators.minLength(6), Validators.required]),
      
      address: new FormGroup({
        country: new FormControl('by'),
        city: new FormControl('', [Validators.required])
      }),
      
      skills: new FormArray([])
    })
  }
```

## Валидатор (validator)
Валидатор (validator) - это функция которая возращает функцию (ValidatorFn) которая получает control и синхронно возвращает карту ошибок проверки, если они есть, в противном случае — null.
  
Валидаторы (validator) для форм бывают:
  
  * встроенные (built-in) - required, email, pattern и minLength;
  * пользовательский (custom validators);  
  и
  * async;
  * sync;

Есть много встроенных (built-in) валидаторов из класса Validators. Встроенный класс имеет следующие методы валидации:  
  * required;
  * minLength();
  * maxLength();
  * email;
  * **pattern**
  * и т.д.

Взаимодействие с валидаторами состояния INPUT.
Валидаторы имеют следующие состояния:  
* pristine - когда еще ничего не вводили;
* dirty - когда что то ввели;
* touched - когда было переведено в focus;
* untouched - когда не было переведено в focus, не тронуто;
* valid - когда пройдена валидация;
* invalid - когда не пройдена валидация;
* pending - когда ждем ответа от валидатора

## Создание простого пользовательского (custom validators)
Example sync custom validator:
```
  static restrictedEmail(control: FormControl): {[key:string]: boolean} | null {
    if (['test@gmail.com', 'test2@gmail.com']) {
      return {restrictedEmail: true}
    }
    return null
  }
```

Example async custom validator:
```
  static uniqEmail(control: FormControl): Promise<any> | Observable<any> || null {
    return new Promise()
  }
```
