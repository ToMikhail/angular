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

Взаимодействие с валидаторами состояния INPUT (Статусы состояния(Кдассы в html в браузере)).
Валидаторы имеют следующие состояния:  
* pristine - когда еще ничего не вводили;
* dirty - когда что то ввели;
* touched - когда было переведено в focus;
* untouched - когда не было переведено в focus, не тронуто;
* valid - когда пройдена валидация;
* invalid - когда не пройдена валидация;
* pending - когда ждем ответа от валидатора

## Создание простого пользовательского (custom validators)

Для создание пользовательского валидатора необходимо создать функцию, т.к. валидатор это всегда функция (можно создовать в компоненте, или в самом класса компонента, либо вынести в отдельный файл в отдельный класс).

1. Example ***sync*** custom validator:
```
  static restrictedEmail(control: FormControl): {[key:string]: boolean} | null {
    if (['test@gmail.com', 'test2@gmail.com']) {
      return {restrictedEmail: true}
    }
    return null
  }
```

2. Example ***async*** custom validator:
```
  static uniqEmail(control: FormControl): Promise<any> | Observable<any> || null {
    return new Promise()
  }
```
Async Validators создаются, например, для проверки Email на сервере (существует такой или нет).  
Async Validators передаются в FormControl 3 парпметром.
For example:
```
  ngOnInit(): void {
    this.form = new FormGroup({
      email: new FormControl('', [
        Validators.email,
        Validators.required
      ]),
      [MyValidators.uniqEmail]
    )
  }
```

## Методы для взаимодействия с Controls

Основные поля объекта реактивной формы Angular:  
- controls — поля, включая вложенные FormGroup;
- errors — содержит ошибки валидации;
- status — строка, определяющая правильность заполнения формы, значение либо VALID, либо INVALID;
- valid — true, если форма валидна;
- invalid — true, если форма невалидна;
- pristine — true, если не было взаимодействия с полями;
- touched — true, если одно из полей становилось активным (получало фокус);
- dirty — true, если пользователь заполнил хотя бы одно из полей;
- value — значение формы в виде объекта;
- ***statusChanges*** — позволяет отслеживать изменение статуса валидности;
- ***valueChanges*** — позволяет отслеживать изменение значения.

Метод ***get()***. Реактивные формы позволяют обращаться к отдельному полю используя метод get(), которому передается в виде строки наименование поля(form.get('email')).

***valueChanges***.Отслеживание изменений формы осуществляется через подписку на valueChanges Observable. Функция обработчик принимает параметром значение формы.  
For example: 
```
this.form.valueChanges.subscribe((v) => {
  console.log(v)
})
```
Использовать valueChanges можно применительно к отдельному полю.   
For example: 
```
this.form.get('login').valueChanges.subscribe((v) => {
  console.log(v)
})
```

***statusChanges***. Для отслеживания изменения статуса поля или формы в целом "подписывайтесь" на ***statusChanges***.
```
this.form.statusChanges.subscribe((status) => {
  console.log(status)
})
```

***reset()***. Для сброса значений полей формы или полей одной из ее групп используется метод reset(), который принимает объект с начальным значением.

```
//Всем полям будет присвоено null
this.form.reset()
//Полю login будет присвоено 'default_login', остальным - null
this.form.reset({ login: 'default_login' })
```

Для динамического изменения структуры Angular reactive forms предусмотрен ряд методов:

1. ***addControl(name: string, value: any)*** — добавляет новое поле соответствующей группе;
1. ***setControl(name: string, value: any)*** — заменяет уже существующее поле соответствующей группы;
1. ***removeControl(name: string)*** — удаляет поле из группы.
1. ***patchValue()*** и ***setValue()***
Для задания форме значений, например, при редактировании данных, полученных от сервера, используются методы patchValue() и setValue().

***setValue()***. Методу setValue() должен передаваться объект, полностью совпадающий по строению с описанной моделью формы, а patchValue() — лишь часть этой структуры.
```
this.loginForm.patchValue({ login: 'user123' })
this.loginForm.setValue({
  login: 'user123',
  password: 'pwd123',
})

```
Если setValue() передать "неполную" модель, будет сгенерирована ошибка.

Вторым параметром оба метода принимают объект, с помощью которого, например, можно сделать так, чтобы установка значения Angular reactive forms не инициировала событие valueChanges.
```
this.form.patchValue(
  { login: 'user123' },
  { emitEvent: false }
)
```

Для того что бы связать form c html необходимо добавить [ngFormGroup]="form", где ащкь переменная в модели (.ts). И так же необходимо добавить (ngSubmit)="submit()" - для отправки и получения данных из формы. 

For example:  
example.component.html
```
<div class="container">
  <form class="card" [formGroup]="form" (ngSubmit)="submit()">
    <h1>Forms and validation</h1>

    <div class="form-control">
      <label>Email</label>
      <input type="text" placeholder="Email" formControlName="email">

      <pre>{{ form.get('email')?.errors | json }}</pre> // просто для вывода информации (не нужно писать) 

      <div class="validation"
        *ngIf="form.get('email')?.invalid && form.get('email')?.touched">
        <small *ngIf="form.get('email')?.errors?.required">can't be empty</small>
        <small *ngIf="form.get('email')?.errors?.email">enter correct Email</small>
        <small *ngIf="form.get('email')?.errors?.restrictedEmail">enter it is prohibited</small>
      </div>
    </div>

    <div class="form-control">
      <label>Password</label>
      <input type="password" placeholder="Password" formControlName="password">

      <pre>{{ form.get('password')?.errors | json }}</pre>

      <div class="validation"
      *ngIf="form.get('password')?.invalid && form.get('password')?.touched">

        <small *ngIf="form.get('password')?.errors?.required">can't be empty</small>
        <small *ngIf="form.get('password')?.errors?.minlength">
          length should be more than {{ form.get('password')?.errors?.minlength.requiredLength}}. 
          Now is {{ form.get('password')?.errors?.minlength.actualLength }}
        </small>

      </div>
    </div>

    <div class="card" formGroupName="address">
      <h2>Address</h2>

      <div class="form-control">

        <label>Country</label>
        <select formControlName="country">
          <option value="by">Belarus</option>
          <option value="ru">Russia</option>
          <option value="ua">Ukraine</option>
        </select>
      </div>

      <div class="form-control">
        <input type="text" placeholder="City" formControlName="city">
      </div>

      <button class="btn" type="button"  (click)="setCapital()">Choose capital</button>
    </div>


    <div class="card" formArrayName="skills">

      <h2>Your skills</h2>
      <button class="btn" type="button" (click)="addSkill()">Add skill</button>

      <div class="form-control"
        *ngFor="let control of skills.controls; let i = index">
        <label>Skill {{ i + 1 }}</label>
        <input type="text" [formControlName]="i">
      </div>
    </div>

    <button class="btn" type="submit" [disabled]="form.invalid">Submit</button>
  </form>
</div>
```
example.component.ts
```
export class FormsAndValidationsComponent implements OnInit {
  form: FormGroup

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

  submit() {
    this.form.reset();
  }
  setCapital() {
    type NewType = {
      [key: string]:string
    }

    const cityMap: NewType = {
      by: 'Minsk',
      ua: 'Kiev',
      ru: 'Moskva'
    }

    const cityKey:string = this.form.get('address')?.get('country')?.value
    const city = cityMap[cityKey]

    this.form.patchValue({address: {city: city}})
  }

  addSkill(): void {
    const control = new FormControl('', Validators.required);
    // (<FormArray>this.form.get('skills')).push(control)
    (this.form.get('skills') as FormArray).push(control)
  }

  get skills() {
    return (this.form.controls['skills'] as FormArray)
  }
}
```
Что бы связать поля формы модели (.ts) и шаблона (.html) необходимо добавить formControlName="name"(formGroupName="name") <= как в модели.  
Для того что бы полчить данные формы используется свойство this.form.value.  
