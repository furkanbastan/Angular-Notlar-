### Angular Guards Yapılanması
    - angularda rotalara erişim izni vermek veya reddetmekle alakalı
    - oturum işlemleri, kimlik doğrulama işlemleri vs buradan yapılır
    - 5 adet guard yapısı mevcuttur
    - CanActivate, CanDeactivate, Resolve, ̶C̶a̶n̶L̶o̶a̶d̶(CanMatch), CanActivateChild
    - CanActivate => ilgili rotanın etkinleştirilip etkinleştirilmeyeceğini karaar verir
    - CanActivateChild => child routelara erişilip erişilmeyeceğine karar verir
    - Resolve => bazı görevler tamamlanana kadar rotanın etkinleştirilmesini geciktirir, API'dan verilerin çekilip hazırlanması gerektiği durumlarda kullanılır
    - CanLoad => lazy loading ile yüklenecek modüllerin yüklenmesini kontrol eder gerekirse engeller
    - CanDeactivate => geçerli rotadan çıkılmak istendiğindeki durumu kontrol eder, genelde kaydedilmemiş veriler olduğunda kullanılır
    - CanMatch => bu guard ile aynı rotayı farklı şartlarla erişilebilir, mesela boş path giriş yapılmışken farklı giriş yapılmamışken farklı olabiliyor
```javascript
[
    {path: '', component: LoggedInHomeComponent, canMatch:{IsLoggedIn}},
    {path: '', component: HomeComponent},
]
```
    - Angulardaki guard yapılanmalır deprecate edildi ama yinede kullanılabiliyor
    - Bunlar yerine daha idealleştirilmiş bir yöntem olan Functional Router Guard yaklaşımı kullanılır
    - peki nasıl guard oluşturulur => cli ile guard oluşturulur, kodlar yazılır, import edilir, route'a uygun property ile üzerinden eklenir
        - ng g g guard/example    =>   @Injectable ile işaretlenmiş bir fonksiyon gelir yani özünde bir servistir
        - default olarak providedIn root olarak geldiğinde import etmeye gerek yoktur
```javascript
{path: 'products', component: ProductComponent, canActivate:[ExampleGuard]}
```
#### CanActivete && CanActivateFn
    - Bir kullanıcının ilgili path'e erişim izni olup olmadığını kontrol etmek için kullanılır
    - CanActivate yerine direkt CanActivateFn türünden fonksiyon olarak tanımlmayıp direkt fonksiyon olarak kullanmaktayız
```javascript
export const canActivateFnExample : CanActivateFn = (route : ....) => {return true;}
```