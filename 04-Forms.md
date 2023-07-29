## Angular Forms
- template-driven forms ve model-driven forms(Reactive Forms) olarak iki formu vardır
- template-driven forms:
    - form elemanları ngModel, form ise ngForm direktifi ile işaretlenir
    - ngForm direktifi arka planda FormGroup nesnesi oluşturur
- model-driven forms:
    - dinamiktir, componentte nesne/object tanımlanır ve HTML'de form etiketlerine bind edilir
---------------------
### Template-Driven Forms
-----------------
#### formlarda kullanılan fonksiyonlar
- FormGroup  =>  formun kendisini temsil eder
- FormArray  =>  dinamik olarak kontrolleri temsil eder
- FormControl  =>  kullanıcıdan veri alacağımız alanlardır, FormControl sabit bir form bölümünü temsil ederken FormArray dinamik bir alanı temsil eder
- FormBuilder  =>  FormGroup, FormArray, FormControl elemanlarını oluşturmayı kolaylaştıran servistir
-------------------
#### model-driven forms ile template-driven forms farkları
- model-dr forms verileri bir veri modeli ile yürütür ama template-dr forms verileri html templateleri ile yönetir
- model-dr forms tüm kontrolleri doğrulama kurallarına tabi tutar ama template-dr forms ngModel ile işaretlenmiş templateleri doğ. kurallarına tabi tutar
- model-dr forms kontrol bağımsızlığını sağlar(bir form kontrolünü birden fazla formda kullanabilmek)
- model-dr forms test edilebilirlik açısından daha iyidir
- model-dr forms büyük formları daha iyi yönetir, template-dr forms ise basit formlar için daha uygun
--------------
#### template-driven forms örnekleri
- önce FormsModule import edilmeli
```javascript
//template kısmında
<form #frm="ngForm" (ngSubmit)="frm.value">
    <input type="text" name="name" ngModel>
    <select name="job" ngModel>
        <option value=0> Teacher </option>

//component kısmı
@ViewChild("frm", {static=true}) frm : NgForm;
ngOnInıt(): void {
    console.log(this.frm)
    console.log(this.frm.form)
    console.log(this.frm.controls)
}
onSubmit(data : {name:string, job:number}){
    console.log($´{data.name} adlı kullanıcının mesleği: {data.job}´);
}
```
----------------
#### ngForm direktifi detayları
- value  =>  her controle nesnesinin değerini getirir
- valid  =>  form geçerliyse true değilse false döndürür
- touched  =>  kullanıcı formda en az bir değer girdiyse true
- submitted  =>  form submit edildiğinde true döner
---------------------
#### FormControl detayları
- value  =>  
- valid  =>
- invalid  => 
- touched  =>  herhangi bir değer girdiyse
-------------------------
#### ngModelGroup direktifi
- bu direktif birden fazla form kontrolünü gruplamak için kullanılır
- ilgili kontroller ilişkilendirilip ortak bir işlevsellik oluşturulabilir
```htm
<div ngModelGroup="adress">
<input type="text" name="city" ngModel>
```
----------------------
#### form kontrollerine ilk değer atama
- setTimeout kullanılma sebebi OnInıt eventi fırlatıldığında form henüz initialize edilmemiş olması
```javascript
ngOnInit(): void{
    setTimeout(()=>{
        this.frm.setvalue({
            name :{
                first:"Furkan",
                last:"Baştan"
            },
            job:"mühendis"
        })
    },200);
}
this.frm.controls["name"].setValue("Furkan");  //setTimeout içinde
```
- formun belirli bir kısmına ilk değer atamak için  =>  this.frm.control.patchValue({surname:"Baştan"})
--------------------------
### Model-Driven Forms(Reactive Forms)
- ReactiveFormsModule import edilmeli
- Oluşturulacak formun önce FormGroup türünden modeli oluşturulur sonra FormController tanımlanır, bunlar için FormBuilder dan istifade edinilir
```javascript
//component kısmında
export class AppComponent{
    frm : FormGroup;

    constructor(private formBuilder : FormBuilder){
        this.frm = formBuilder.group({
            name : ["Furkan"],
            surname : [""],
            email : [""],
            tel : [""],
        });
    }
}

//template kısmında
<form [formGroup]="frm" (ngSubmit)="onSubmit(frm.value)">
    <input type="text" formControlName="name">
    <input type="text" formControlName="surname">
    <input type="email" formControlName="email">
    <input type="text" formControlName="tel">
</form>

//component kısmı
onSubmit(data : {name: string, surname: string, email: string, tel: string}) {}
```
- mesela name kısmına default değer olarak "Furkan" yazılmıştır
--------------
#### formGroupName ile gruplama
```javascript
//component tarafında
this.frm = formBuilder.group({
    name: [""],
    adress: formBuilder.group({
        country: [""],
        city: [""]
    })
});

//template tarafında
<div formGroupName="adress">
    <input type="text" formControlName="country">
    ....
</div>
```
------------------
#### validation tanımlama
```javascript
this.frm = formBuilder.group({
    name: ["", Validators.required],
    surname: ["", [Validators.required, Validators.max(500)]]
    })
});
```
----------------
#### onlySelf özelliği
- bir form kontrolünün değeri değiştiğinde tüm kontrollerin validasyon kontrolü değilde sadece ilgili kontrolün sorumluluğunu alır
```javascript
this.frm.controls["name"].setValue("Furki");  //bu kod tüm formun geçerliliğini kontrol ettirir ve valid özelliğinin değeri değişir
this.frm.controls["name"].setValue("Furki", {onlyself: true});  //ama bu kod ilgili kontrolün geçerliliğini kontrol ettirir ve valid değeri değişmez
```
--------------
#### value changes & status changes
- value changes => formdaki herhangi bir kontrolün değeri değiştiğinde fırlatılır
- status changes => formun değeri değiştiğinde fırlatılır
```javascript
this.frm.valueChanges.subscribe({
    next: data => {
        console.log(data);
    }
});
```