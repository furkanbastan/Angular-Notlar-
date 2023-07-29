### Changing The Form Status
- angularda form yapılanmasının durumunu değiştirmemizi sağlayan fonksiyonlar vardır
- markAsTouched  markAsUnTouched  markAllAsTouched
-------------------
#### markAsTouched
    - bir formda veya kontrolde işlem yapılırsa formun ve ilgili kontrolün touched özelliği true olacaktır
    - fonksiyonda onlyself parametresi true verilirse sadece o yapının touched özelliği değişir 
```javascript
this.frm.markAsTouched();
this.frm.get("name").markAsTouched();
this.frm.get("name").markAsTouched({onlyself: true});
//biri form için biri kontrol için, bu fonksiyonlar ture/false döndürür
```
----------------------
#### markAllAsTouched
    - o kontrolün ve altındaki kontrollerin touched özelliğini değerini true olarak değiştirecektir
```javascript
this.frm.get("address").markAsTouched();    //address için touched değeri true iken country için touched değeri false dur
this.frm.get("address").markAllAsTouched();  //ama burada hem address için hem de country için touched değeri true dur
```
-----------
#### markAsUntouched
    - bu fonksiyon ile ilgili form veya kontrolün touched özelliği false olarak ayarlanacaktır
```javascript
this.frm.markAsUntouched();
```
-------------
#### markAsDirty
    - ilgili formun veya kontrolün dirty değeri programlamatik olarak değiştirilebilecektir
    - forma dokunduğunda touched true olur değer girmeye başladığında dirty true olur
```javascript
this.frm.markAsDirty();
this.frm.get("name").markAsDirty();
```
-------------------
#### markAsPristine
    -  ilgili formun pritine değerini true olarak ayarlar böylece forma hiç dokunulmamış gibi gözükür
```javascript
this.frm.markAsPristine();
this.frm.get("name").markAsPristine();
```
--------------
#### disable & enable fonksiyonları
    -formun veya kontrolün aktif veya devre dışı olmasını sağlarlar
```javascript
this.frm.disable();
this.frm.enable();

this.frm.get("name").disable();
```