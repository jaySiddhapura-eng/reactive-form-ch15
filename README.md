# Reactive Forms

## Table of Contents
* [Introduction](#Introduction)<br>
* [Setup](#Setup)<br>
* [Adding Form Control](#Adding-Form-Control)<br>
* [Connecting Above Created FormControl to The HTML Template](#Connecting-Above-Created-FormControl-to-The-HTML-Template)<br>
* [Submit The Form](#Submit-The-Form)<br>
* [Validating The Input Data](#Validating-The-Input-Data)<br>
* [Getting Access to Controls](#Getting-Access-to-Controls)<br>
* [Grouping The FormControls](#Grouping-The-FormControls)<br>

## Introduction

## 	[:arrow_up:](#Table-of-Contents )<br>

1. In template drive n approach of form implementation, we first created the form template and then we connect that template to the typescript object and code
2. In Reactive form implementation we first create form object in typescript and then we uses this form to create template in HTML 

## Setup

## 	[:arrow_up:](#Table-of-Contents )<br>

1. Import the ```FormGroup``` in the ```app.component.ts```

   ~~~typescript
   // app.component.ts
   import {FormGroup} from '@angular/forms';
   export class AppComponent {
       signupForm: FormGroup;
   }
   ~~~

2. Register form group in ```app.module.ts```

   ~~~typescript
   @NgModule({ 
   	  imports: [ ReactiveFormsModule ]
   })
   ~~~

## Adding Form Control

## 	[:arrow_up:](#Table-of-Contents )<br>

~~~typescript
//app.component.ts

export class AppComponent {
    signupForm: FormGroup;
    
    ngOnInit(){
        this.signupForm = new FormGroup({ 
            'username':new FormControl(null),	// default value = null
            'email' : new FormControl(null),	
            'gender' : new FormControl('male')	// default value = 'male'
        })
	}
}
~~~

## Connecting Above Created FormControl to The HTML Template

## 	[:arrow_up:](#Table-of-Contents )<br>

~~~html
<form [formGroup]="signupForm">
    
    <div class="form-group">
    	<label for="username">Username</label>
    	<input type="text"
               id="username"
               formControlName="username"
               class="form-control">
    </div>
    
    <div class="form-group">
         <label for="email">email</label>
         <input type="text"
                id="email"
                formControlName="email"
                class="form-control">
    </div>
    
    
    <div class="radio" *ngFor="let gender of genders">
          <label>
          <input type="radio"
                 formControlName="gender"
                 [value]="gender">{{ gender }}
          </label>
   </div>

</form>
~~~

## Submit The Form

## 	[:arrow_up:](#Table-of-Contents )<br>

1. Technically form is already submitted and .ts has all the form control inputs already

   ~~~html
   <form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
   </form>
   ~~~

   ~~~typescript
   export class AppComponent {
      signupForm: FormGroup;
      onSubmit(){
       console.log(this.signupForm);
       this.signupForm.reset();
   	} 
   }  
   ~~~

## Validating The Input Data

## 	[:arrow_up:](#Table-of-Contents )<br>

1. FormControl support some default validators

2. You can apply one validator or multiple validator to the individual formControls

   ~~~typescript
   //app.component.ts
   
   export class AppComponent {
       signupForm: FormGroup;
       
       ngOnInit(){
           this.signupForm = new FormGroup({ 
               // one validator
               'username':new FormControl(null,Validators.required),	
               // multi validator	
               'email' : new FormControl(null,[Validators.required, Validators.email]), 
               'gender' : new FormControl('male')	
           })
   	}
   }
   ~~~

## Getting Access to Controls

## 	[:arrow_up:](#Table-of-Contents )<br>

~~~html
<div class="form-group">
     <label for="username">Username</label>
     <input ...>
    
    <!--access in individual form control-->
	<span *ngIf="!signupForm.get('username').valid &&
            	 signupForm.get('username').touched">
    </span>

</div>

<!--.get keyword can not be used if you are accessing form control outside its divs-->
~~~

## Grouping The FormControls 

## 	[:arrow_up:](#Table-of-Contents )<br>

~~~typescript
// app.component.ts
 ngOnInit(){ 
    this.signupForm = new FormGroup({
       'userData': new FormGroup({
             'username':new FormControl(null, Validators.required),
             'email' : new FormControl(null, [Validators.required, Validators.email])
              }),
       'gender' : new FormControl('male'),
       'hobbies' : new FormArray([])
    });
 }

// userData is grouped form control and holds username and email
~~~

~~~html
<!--app.component.html-->
<form>
    <div formGroupName="userData">
             <label for="username">Username</label>
     		<input ...>
    
    		<!--access in individual form control-->
			<span *ngIf="!signupForm.get('userData.username').valid &&
            	 		 signupForm.get('userData.username').touched">
    		</span>
    </div>
</form>
~~~








