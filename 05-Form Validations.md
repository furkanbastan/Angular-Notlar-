### Form Validations
    - Formun validasyonu bütünsel olarak sağlanmadığı sürece formun valid değeri false olarak döner
    - Validation'lar yapısal olarak sync validators ve async validators olarak ikiye ayrılırlar
        - sync validator doğrulamaları hemen çalıştırır ve hemen ardından geriye sonuç döner, hata listesi veya null döndürür
        - async validator da ise doğrulamalar Promise veya Observable döner ve subscribe olunabilir bir davranış sergilerler
--------------
#### built-in validators
    - angularda ReactiveForms modülü ile built-in gelen validatorslar:
        - min                      => değer eşit veya büyük olmalı
        - max                      => değer eşit veya küçük olmalı
        - required                 => değer girilmesi zorunlu olmalı
        - requiredTrue             => girilen değer true olmalı, genelde checkBox'larda kullanılır
        - email                    => değer email formatında olmalı
        - minlength & maxlength    => değer metinsel değerde karakter sınırı belirlenir
---------------
#### doğrulama ve hata mesajları
    - FormControl nesnelerinin errors propertysi'den istifade edeceğiz
```javascript
//component kısmında name için property oluşturalım
get name(){ return this.frm.get("name"); }

//template kısmında
<input type="text" formControlName="name">
<div *ngIf="!name.valid && (name.dirty || name.touched)">{name.errors | json}</div>
```
----------------
#### custom validator oluşturma ve kullanma
    - ValidatorFn interface inden istifade edilir
```javascript
//custom validator tanımlamak için aşağıdaki fonksiyonu declare etmek gerekir
export declare interface ValidatorFn{
    (control: AbstractControl): ValidatoinErrors | null;
}

export function capitalLetterValidator() : ValidatorFn{
    return (control: AbstractControl): ValidationErrors | null => {
        const value = control.value;

        const ascii: string[] = [];
        for(let n = 65; n <= 90; n++){
            ascii.push(String.fromCharCode(n));
        }

        if(ascii.indexOf() == -1)
            return {capitalLetter: true};
        
        return null;
    }
}

name: ["", [Validators.Required, capitalLetterValidator]]
```
----------------
#### parametreli custom validator
```javascript
export function capitalLetterValidator(ccount: number) : ValidationErrors {
    return (control: AbstractControl): ValidationErrors | null => {
        const value = control.value;

        const ascii: string[] = [];
        for(let n = 65; n <= 90; n++){
            ascii.push(String.fromCharCode(n));
        }

        if(ascii.indexOf() == -1)
            return {capitalLetter: true};
        
        return null;
    }
}

name: ["", [Validators.Required, capitalLetterValidator(3)]]
```
--------------
#### async validator
    - şu ana kadar gördüğümüz tüm validationlar sync validation idi
    - AsyncValidatorFn interface'inden istifade edeceğiz
    - genelde doğrulama için gerekli verilerin dış servisten alındığı uzun soluklu süreçler için kullanılmaktadır
    - bunun örneği 16. videoda
```javascript
export declare interface AsyncValidator{
    (control: AbstractControl): Promise<ValidationErrors | null> | Observable<ValidationErrors | null>;
}
```
-------------
#### karşılaştırma validatörleri
    - iki kontrolü karşılaştırmak istiyor olabiliriz, örneğin başlangıç ve bitiş tarihleri
    - bu kontroller için tekil bir validator oluşturmamız gerek bunun için FormControl yerine FormGroup kullanılmalı
    - parametrik hale de getirilebilir
```javascript
constructor(formBuilder: FormBuilder) {
    this.frm = formBuilder.group({
        password: ["", Validators.required],
        confirmPassword: ["", Validators.required]
    },
    {
        validators: [matchPassword()]
    });
}

export function matchPassword(): ValidatorFn{
    return (control: AbstractControl): ValidationErrors | null => {
        const password = control.get("password").value;
        const confirmPassword = control.get("confirmPassword").value;
        if(password != confirmPassword)
            return {"noMatch": true};
        return null;
    }
}
```
-------------------
#### dinamik olarak validator eklemek & silmek
    - setValidators & setAsyncValidators fonksiyonları kullanılabilir
    - setValidator ile eklenen validator diğer eklenen validatorların üzerine yazılır
    - ve bu metot ile önceden eklenmiş tüm sync & async validatorlar kaldırılmış olacak
    - clearValidators ile bir kontrolden tüm validatorları kaldırabiliriz
```javascript
this.frm.get("password").setValidators(Validators.maxLength(3));
this.frm.get("password").clearValidators();
```
--------------------------
#### validator durumlarını güncelleme(dinamik eklenip kaldırılan)
    - validatorlar sadece alanın değerini değiştirdiğimizde çalışırlar, formun ve kontrolün geçerlilik durumunu hemen değiştirmezler
    - updateValueAndValidity ile angular mimarisini validatorların çalışması için zorlayabiliriz
```javascript
this.frm.get("name").updateValueAndValidity();
this.frm.updateValueAndValidity();
```