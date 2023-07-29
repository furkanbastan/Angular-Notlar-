## Directives
- ngFor, ngSwitch, ngIf, ngClass, ngStyle, mgModel, NgNonBindable
-------------------------
#### ngFor direktifi
- index, first, last, even, odd kullanılabilir
```javascript
<li *ngFor="let name of names">{{name}}</li>
<li *ngFor="let name of names; index as i">{{name}}</li>
<li *ngFor="let name of names; let i = index;">{{name}}</li> 
```
-------------------------
#### ngIf direktifi
```javascript
<div *ngIf="visible"></div>
<div *ngIf="visible; else elseMessage"></div> => <ng-template elseMessage>bu ne bucum mezajdir</ng-template>
```
---------------------
#### ngSwitch direktifi
```javascript
<div [ngSwitch]="sayi">
  <div *ngSwitchCase="1">sayı 1 dir</div>
  <div *ngSwitchCase="2">sayı 2 dir</div>
  <div *ngSwitchDefault>sayı mayı yok gardaş</div>
</div>
```
----------------------
#### ngClass, ngSwitch direktifleri
```javascript
//component kısmında
cl :string = "myclass";
style: any ={
    'background-color': 'grey',
    'color': 'light'
}

//template kısmında
<div [ngClass]="cl"></div>
<div [ngStyle]="style"></div>
```
---------------------------
### Custom Directives
- ng g d directives/...
- directive'e parametre aktarma konusu
- HostListener, HostBinding konuları
- bir directive oluşturulduğunda ana modülde declare edilmeli(otomatik ediyor)
-----------------------
#### selector attribute yani [..] şeklindeyse
```javascript
//component kısmında
@Directive({
  selector: '[appDeneme]'
})
esport class DenemeDirective{...}

//template kısmında
<div [appDeneme]></div>
```
-------------------
#### selector .bilmemne şeklindeyse
```javascript
//component kısmında
@Directive({
  selector : '.appDeneme'
})
export class DenemeDirective{...}

//template kısmında
<div appDeneme></div>    => bu terih edilir
<div class="appDeneme"></div>
```
---------------------
#### direktif ile işaretlenmiş elementi elde etmek
```javascript
@Directive({
  selector: '[appDeneme]'
})
export class DenemeDirective{
  constructor(private element :ElementRef){element.nativeElement.style.display='none';}
}
```
----------------
#### direktif ile parametre yakalamak
```javascript
<div [appDeneme] color="blue"></div>
@Input() color: string;
```
--------------------
#### HostListener ile directive'in hangi event ile görevlendirileceğini belirlemek
```javascript
//component kısmında
@HostListener("click")
onClick() { alert("HTML nesnesi click edildi.") }

//template kısmında
<div [exampleDirective]></div>
```
----------------
#### HostBinding ile html elementinin bir özelliğini bind etmek
```javascript
@HostBinding("style.color")
writingColor :string = "blue";
```
----------------------
#### custom structural direktives
- default structural direktifler(* ile başlarlar) html template yapısını değiştirirken(view cont ref ile) normal directiveler html elemanlarına ekstra özellikler eklerler
- structural direktiflerde iki modül => TemplateRef ViewContainerRef(dinamik component loading)
```javascript
//if için custom struc directive
@Directive({
  selector: '[appIfDeneme]'
})
export class IfDenemeDirective{
  constructor(
    private templateRef : TemplateRef<any>,
    private viewContainerRef : ViewContainerRef
  ){}
  @Input() set appIfDeneme(value : boolean){
      if(value) this.viewContainerRef.createEmbeddedView(this.templateRef)  //DOM'a ekler
      else this.viewContainerRef.clear()  //DOM'dan kaldırır
    }
  }

<div *appIfDeneme="true"></div>  bu element gözükür
<div *appIfDeneme="false"></div>  bu element gözükmez
```

------------------------
#### yorum
- anladığım kadarıyla direktifte * olunca fonksiyona parametre aktarıyosun [..] olunca değişkene parametre aktarıyorsun mesela:
```javascript
<xxx *appDirektif="selam">
@Input() set appDirektif(value :string){......}

<xxx [appDirektif] value="selam">
@Input() value : string;
```