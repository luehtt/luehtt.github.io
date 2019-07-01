# How to use Angular 6 Reactive form

Reactive forms provide a model-driven approach to handling form inputs whose values change over time. This guide shows you how to create and update a simple form control, progress to using multiple controls in a group, validate form values, and implement more advanced forms.

### Step 1: Register the reactive forms module
To use reactive forms, import **ReactiveFormsModule** from the **@angular/forms** package and add it to **app.module.ts** imports array.
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [ AppComponent ],
  imports: [ BrowserModule, AppRoutingModule, HttpClientModule, ReactiveFormsModule ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Step 2: Open the component and declare form and formBuilder
The **FormControl** class is the basic building block when using reactive forms. To register a single form control, import the **FormBuilder**, **FormGroup**, **Validators** class into your component and create a new instance of the form control to save as a class property.
In this example, the form consists of 5 form controls, namely username, email, birthdate, and phone. The username, email, and birthdate form controls are **required**.
```
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

export class ReactiveFormComponent implements OnInit {
    form: FormGroup;
    
    constructor(private formBuilder: FormBuilder) { }
    
    ngOnInit() {
      this.form = this.formBuilder.group({
        username: ['', [Validators.required]],
        email: ['', [Validators.required, Validators.email]],
        phone: [''],
    });
  }
}
```

### Step 3: Open the view and construct the form in html
After you create the control in the component class, you must associate it with a form control element in the template. Update the template with the form control using the formControl binding provided by **FormControlDirective** included in **ReactiveFormsModule**.
For each form controls, there is a **error** div right after to hinder the user on submit.
```
<form *ngIf="form" [formGroup]="form" (ngSubmit)="onSubmit()">
    <div class="form-group row">
        <label class="col-sm-4">Username</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" formControlName="username" />
            <div class="text-danger" *ngIf="form.controls.username.touched && form.controls.username.errors">
            <small *ngIf="form.controls.username.errors.required">This username form control is required</small>
            </div>
        </div>
    </div>
    <div class="form-group row">
        <label class="col-sm-4">Email</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" formControlName="email" />
            <div class="text-danger" *ngIf="form.controls.email.touched && form.controls.email.errors">
            <small *ngIf="form.controls.email.errors.required">This email form control is required</small>
            <small *ngIf="form.controls.email.errors.email">This email form control must be an email</small>
        </div>
    </div>
    <div class="form-group row">
        <label class="col-sm-4">Email</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" formControlName="email" />
            <div class="text-danger" *ngIf="form.controls.email.touched && form.controls.email.errors">
            <small *ngIf="form.controls.email.errors.required">This email form control is required</small>
            <small *ngIf="form.controls.email.errors.email">This email form control must be an email</small>
        </div>
    </div>
    <div class="form-group row">
        <label class="col-sm-4">Phone</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" formControlName="phone" />
        </div>
    </div>
</form>
```
### Step 4: Get form controls value and submit
Before submitting the form controls value to the server, the **Validators** must confirm all the form controls are valid. The **touched** properties after the form control name is to mark if the user omit the field or else all the form controls are marked danger as invaid at initial stage before user can actually touch the field. Therefore, on validating the form, we must touch all field so that they are marked touched.
```
onSummit() {
    if (this.form.invalid) {
        for (let i in this.form.controls) {
            this.form.controls[i].markAsTouched();
        }
        return;
    }
    
    let item = {
        username = this.form.controls.username.value;
        email = this.form.controls.email.value;
        address = this.form.controls.address.value;
    }
    
    // this service calling is just example on posting new item
    this.service.post(item).then(res => {
        // codes on success
    })
    .catch(err => {
        // codes on error
    });
}
```

