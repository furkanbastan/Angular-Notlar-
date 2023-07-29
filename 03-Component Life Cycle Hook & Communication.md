## Component Communication
#### parent-to-child inceleyelim
```javascript
//template kısmında
<p>Parent works!</p>
<app-child chlidData="iste veri"></app-child>

//child componentte
@Input() chlidData: any;
```
----------------
#### child-to-parent inceleyelim
```javascript
//child component içinde
@Output() dataEvent : EventEmitter<any> = new EventEmitter();
this.dataEvent.emit({message : 'Merhaba'});

//parent template içindeyiz
<p>Parent works!</p>
<app-child (dataEvent)="childEvent($event)"></app-child>

//Parent ts içindeyiz
childEvent(obj :any){console.log(obj)}
```
------------------
#### iki child component arasında ise sanırım önce parenta göndereceğiz sonra parenttan diğer childa göndereceğiz
------------------------------------------------------------------------------------------------------------------------
#### Component Life Cycle Hook
- bir component uygulamada kullanılırken ngOninit tetiklenir
- componentler yaşam döngüsüne göre belirli tepkilerde bulunur
- componentin oluşup silinme sürecinde belirli noktalarda çalışan metotlar bulunmaktadır
  - constructor
    - component ilk oluştuğunda zaten
  - ngOnChanges
    - componenti selector referansı üzerinden Input değişkenleri değiştiğinde tetiklenir
  - ngOnInit
    - component ilk kez oluşturulunca tetiklenir, yani selectorı inputu felan oluşturulduktan sonra 
  - ngDoCheck
    - component üzerinde değişiklik olduğunda tetiklenir, change detection döngüsünde çalışır
  - ngAfterContentInit        => sayfa içeriği yüklendikten sonra
    - <ng-content|ng-content> 
  - ngAfterContentChecked     => safya içeriği değiştikten sonra tetiklenir
  - ngAfterViewInit           => view yüklenince
  - ngAfterViewChecked        => view değiştiğinde
  - ngOnDestry                => componente hoşçakal dediğimiz yer