### Dependency Injection
    - kodsal çalışmaları yeniden kullanabilmemizi sağlayan çağdaş bir design pattern'dır
    - dependency bir yazılım parçasının(class, metot) başka bir bileşene ihtiyaç duymasıdır
    - dependency temel gayesi bağımlılıkları yönetilebilir bir hale getirmek
    - service sınıfı çalışmanın yapıldığı modülde Providers alanına eklenmeli
    - DI aktörleri => consumer, dependency, injection token(DI token), provider, injector
        - consumer => bağımlılığa ihtiyaç duyulan sınıf(component, directive, service)
        - dependency => consumer'da istenilen servis
        - DI token => dependency'leri unique bir şekilde tanımlayan değer
        - provider => injection token eşliğinde dependency'nin tutulduğu yer
        - injector => dependency'leri ihtiyaç halinde kullanmamızı sağlayan yapılardır
        - provider{DI token + dependency}  => injector => consumer{...}
    - Injectable decaratörü ile işaretlenen classlar uygulamanın her yerinde DI ile talep edilebilir, providers eklemeye gerek yok
```javascript
@Injectable({
    providedIn: "root"
})
export class ProductsService{...}
```
---
#### Injector Detayları
    - Injector provider da dependencyleri dı-token ile ilişkilendirip dı-tokenlarını kullanarak elde etmemizi sağlar
    - Angular'da iki injector ağacı vardır : module ınjector tree, element ınjector tree
---
#### @Injectable() decaratörü
    - providedIn parametresi root, any, plartform olabiliyor
        - root olursa servis uygulamada singleton olarak çalışır
        - any olursa servis her modüle karşın instance üretir
        - plartform ise uygulama hem server hem client modunda çalışırken gibi düşünülebilir
---
#### Bir Servisi Provide Etmek
    - bir servisi provide ederken type token, string token, injection token olarak üç tür tokendan istifade edilir