## Data Bindings
- event tetiklendiğinde veya modelin verisi değiştiğinde change detection algoritması kullanılmaktadır
- interpolation, içerisinde sadece field veya property değeri OKUMAK için kullanılır
- =, +=, new, typeof, :, ++, --, bitwise operatörler kullanılamaz
- aritmetik işlemler, string birleştirme, template referansı, fonksiyon tetikleme yapılabilir
- template referansı: <input type="text" #text> {{txt.value}}
- interpolation içinde script/html çalışmaz
- interpolation ile kullanılan: pipes, ?(nullable), !(non null)
#### text interpolation
```javascript
{{title}}
```
--------------------------------------
#### property/attribute binding
```javascript
//template kısmında
<img [src]="title">
<app-home [pageName]="title"></app-home>

//component kısmında
@Input() pageName: string = "";
```
--------------------------
#### event binding
```javascript
//template kısmında
<button (onclick)="event1()"></button>
<button on-click="event1(); event2()"></button>
<input (keydown.shift.a)="eventX()">
```
--------------------------
#### two-way binding
- input nesneleri içerisinde [(ngModel)] direktifi kullanılır
- uygulamanın ana modülünde FormsModule import edilmeli
- ngModelChange  => veride değişiklik olduğunda tetiklenir
```javascript
//component kısmında
title: string = "ilk deger";

//template kısmında
<input [(ngModel)]="title">
{{title}}

<input (ngModelChange)="OnChange($event)">
OnChange($event){..}
```
--------------------------
#### style/class binding
```javascript
<div [style.background-color]="bgColor"></div>
```

## Pipes
```javascript
{{1000 | currency}}                     //$1000
{{1000 | currency : '₺'}}               //₺1000

{{'09.05.2004' | date}}                  //Sep, 09 2004
{{'09.05.2004' | date : 'fulldate'}}     //Saturday, September 5, 2004
{{'09.05.2004' | date : 'medium'}}       //Sep, 09 2004, 12:00:00 AM
{{'09.05.2004' | date : 'shortTime'}}    //12:00 AM
{{'09.05.2004' | date : 'mm:ss'}}        //00:00

{{['e','b','a','y','i','c'] | slice : 2 : 5}}        //a,y,i

{{{name = 'furkan', surname = 'bastan'} | json}}     //{'name':'furkan', 'surname':'bastan'}

{{'furkan bastan' | uppercase}}          //FURKAN BASTAN
{{'Furkan Baştan' | lowercase}}          //furkan bastan

{{50 | percent}}                         //50%

{{'adam sandık fos çıktı' | titlecase}}  //Adam Sandık Fos Çıktı
```
- Custom Pipes
  - ng g p pipes/custom
  - oluşturduğumuz yerdeki modüle otomatik declare edilir