### Angular Routes
    - Önce routes adında bir dizi oluşturalım
```javascript
export const routes: Routes = [
    {path: 'a-component', component: AComponent},
    {path: 'b-component', component: BComponent}
]
```
    - Başına export yazdığımız için module içinde "import {routes} from './routes/router';" şeklide kullanabiliriz
    - Daha sonra Module içinde import edelim
```javascript
@NgModule({
    imports: [RouterModule.forRoot(routes)]
})
```
    - Template de link oluşturuyoruz
```javascript
<a routerLink="a-component">A Component</a>
```
    - Aynı zamanda yeni rotaya uygun componentin yükleneceği alanı router-outlet ile belirtiyoruz
```javascript
<router-outlet></router-outlet>
``` 
```javascript
//routerLink dizi şeklinde de kullanabiliriz
<a routerLink="acomponent/filanca/felance"></a>
<a [routerLink]="['acomponent','filanca','felanca']"></a>
```
```javascript
//routerLinkActive kullanımı
//aktif olan linke dinamik olarak css classı atayabiliyoruz
<a routerLinkActive="aktif">Contact</a>
styles:[".aktif{color:red;}",""]
```
---
#### AppRoutingModule dosyası içinde Route yapılanması
```javascript
const routes: Routes = [
    {path: "", redirectTo: "/home", pathMatch:"full"},
    {path: "home", component: HomeComponent},
]
```
    - redirectTo ile boş olan rotanın home ile default olacağını belirledik
    - burada pathMatch full ise route bire bir aynı olmalı ama mesela "/home/2" rotasını karşılamaz
    - oathMatch prefixdeğeri olur ise "/home/3" gibi bir rotayı karşılar
---
#### WildCart
    - tanımlanmış rotalardan herhangi biriyle eşleşmeyen bir url olduğunda WildCard route devreye girecektir
    - wildcard ** ile temsil edilen bir yapıdır, joker karakterli yol olarakta tarif edilir 
    - tüm rotalar kontrol edildikten sonra eşleşme olmaması halinde wildcard route'a yönlendirme yapılır
    - yani tanımlanmamış bir url'i karşılamak için kullanılır
    - router rotaları sırayla eşler veilk eşleşen rotayı döndürü yani bu en sona yazılmalı!!!
```javascript
{path: "**", component: ErrorComponent}
```
---
#### Location Strategies
    - uygulama url'lerinin  nasıl oluşturulacağı konusunda farklı yaklaşımları ifade eden bir kavramddır
    - uygulamanın farklı sayfalarına istek gönderilirken aşağıdaki konum stratejilerine göre gerçekleştirir
        - HashLocationStrategy => # sembolü kullnır, özellikle eski tarayıcılarda url değişikliklerini algılamasını sağlar
        - PathLocationStrategy =>  normal url ler için, özellikle modern tarayıcılarda tercih edilir, angularda varsayılandır
    - neden bu stratejilere ihtiyacımız var ?
        - angular SPA olduğunda url'i sunucuya göndermez, sayfayı yeniden yükletmez
        - angular urlleri local olarak kullanmakta ve tarayıcı tabanlı davranış sergiler
        -  bu davranış için location strategies'leri kullanır, bunu anlamak için client-side routing anlamak gerek
        - multi-page bir uygulamada uygulamanın bir sayfayı görüntüleyebilmesi için isteği sunucuya göndermesi gerekir
        - angular gibi SPA uygulamada tüm componentler tek bir sayfada(tek bir html sayfasında) görüntülenir
        - dezavantajları:
            - sayfayı tekrar yükleyemezsin
            - url ile sayfayı paylaşamazsın
            - tarayıcıdaki geri düğmesini kullanamazsın
            - SEO kullanmak mümkün değil
        - yani client-side routing tarayıcıda çalışarak sunucu taraflı routingi taklit etmektedir
        - adres çubuğundaki urlyi değiştirir ve tarayıcı geçmişini günceller
        - client-side routing gerçekleştirebilmek için Hash Style Routing veya HTML5 Routing yöntemlerinden biri kullanılabilmektedir
---
#### Route Parameters
    - url üzerinden veri taşımamızı sağlayan yapılardır
    - önce bir parametre tanımlamasında bulunmamız gerekir
```javascript
{path:"products/", component: ProductComponent}
{path:"products/:id", component: ProductComponent}
{path:"products/:id1/:id2/:id3", component: ProductComponent}

<a  [routerLink]="['/products',  product.id]">{{product.name}}</a>
```
#### ActivatedRoute
    - o anda aktif olan rotaylailgili işlemler yapmamızı sağlayan bir nesnedir
    - bu nesne ile urldeki parametrelere ve querystring degerlerine erişebiliyoruz
```javascript
import {ActivatedRoute} from '@angular/router';
constructor(private activatedRoute: ActivatedRoute) {
    const id:any = activatedRoute.snapshot.paramMap.get('id');
    const id:any = activatedRoute.snapshot.params['id'];
    const hasId:any = activatedRoute.snapshot.paramMap.has('id');
}
```
#### Observable ile Url Parametrelerini Okumak
    - ActivatedRoute Observable bir davranış ile route parametrelerini okumamıza yardıcı olur
```javascript
import {ActivatedRoute} from '@angular/router';
constructor(private activatedRoute: ActivatedRoute) {
    activatedRoute.paramMap.subscribe({
        next: param => console.log(param.get('id'))
        veya
        next: param => console.log(param['id'])
    });
}
```
    - peki hangisini kullanmalıyız?
        - angularda aynı rotada ngOnInıt tek bir sefer tetiklenir "../products/1" için tetiklendi "../products/2" için tetiklenmez
        - kısacası observable ile davranış gösteren yapılanmaları kullanmak daha faydalı ve sağlıklı olacaktır
---
#### Child Routes / Nested Routes
    - sd
```javascript
{
    path: "products", component: ProductComponent,
    childeren :[
        {path: "detail/:id", component: ProductDetailComponent}
    ]
}
```
    - angularda child route altına da child route eklenmektedir