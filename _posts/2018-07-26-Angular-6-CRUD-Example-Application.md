---
layout: post
title: Angular 6 CRUD Example Application 
description: Angular 6 CRUD example application - Manage user contacts with local JSON data as service.
date: 2018-07-30 20:18:31+0100
date_modified: 2018-07-30 20:18:31+0100
categories: [angular,typescript,JSON,CRUD,webapp,angular6]
tags:
  - angular
  - typescript
  - JSON
  - CRUD
  - webapp
  - angular6
author: wechris
image:
  path: /img/2018-07-26-Angular-6-CRUD-Example-Application/postpreview.jpg
  width: 629
  height: 600
---

# Angular 6 CRUD Example

My first webapp with Angular 6 step by step from scratch. Generated and modified with angular CLI to manage user contacts with CRUD operations on a local JSON Model.

{% include lightbox.html src="/img/2018-07-26-Angular-6-CRUD-Example-Application/image002.jpg" title="VirtualBox" width="500" %}

## Angular 6 

Requirements: Node 8.9 or higher, together with NPM 5.5.1 or higher. 
For this project, I have npm 6.2.0 and node v9.5.0 installed on my local system.     
    
{% include lightbox.html src="/img/2018-07-26-Angular-6-CRUD-Example-Application/image001.jpg" title="VirtualBox" width="500" %}
    

To install the latest versions of @angular/cli run the command:
    
    
    npm install -g @angular/cli
    

To install a specific version, use `npm install -g @angular/cli@1.4.9`


## Generating Angular 6 Project

The following command generates angular 6 project in the current folder.
    
    
    ng new user-contact
    

The command create a new folder 'user-contact' and a lot of additional stuff.

## Angular 6 Project Structure

{% include lightbox.html src="/img/2018-07-26-Angular-6-CRUD-Example-Application/image005.jpg" title="VirtualBox" width="400" %}

Once the project is generated, chance to the directory and get started with it. Run following commands to see angular 6 app running at http://localhost:4200
    
    
    cd user-contact
    ng serve -o
    
`'-o' is optinal and starts the browser after the build process is finished`

There are certain files generated with CLI command:. 
- `angular.json` File generated contains all the application configuration parameters. The configuration related to welcome html file as index.html, main.ts where all the modules are bundled. You can also find the final application output directory configuration and configuration specific to environment such as dev and prod can be found here.

- `package.json` File contains information about all the project dependencies. 
- `tsconfig.json` for typescript configuration.

Inside the `scr/app` folder we have our components, services, modules, etc. defined.
If the the sources will be changed the angular CLI recognize it and reloads the browser.

The App needs multiple components such as service and components. Following are the commands to generate our components and service.
    
    
    ng g component dashboard
    ng g component add-usercontact
    ng g component edit-usercontact
    ng g serice share/usercontact
    

## Angular CLI Useful Commands
    
    
    ng g component my-new-component
    ng g directive my-new-directive
    ng g pipe my-new-pipe
    ng g service my-new-service
    ng g class my-new-class
    ng g guard my-new-guard
    ng g interface my-new-interface
    ng g enum my-new-enum
    ng g module my-module
    

## Angular 6 - Install Bootstrap

```npm install --save bootstrap```

afterwards, inside angular-cli.json (inside the project's root folder), find styles and add the bootstrap css file like this:

``` css
"styles": [
   "../node_modules/bootstrap/dist/css/bootstrap.min.css",
   "styles.css"
],
```

## Angular 6 Routing

Following is our routing configurtion.We have configured to use LoginComponent as a default component.Also, do not forget to include it in the main module - `app.module.ts`


**app.module.ts**
    
``` typescript 

    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';

    import { AppComponent } from './app.component';
    import { DashboardComponent } from './dashboard/dashboard.component';
    import { UsercontactComponent } from './usercontact/usercontact.component';
    import { AddUsercontactComponent } from './add-usercontact/add-usercontact.component';
    import { EditUsercontactComponent } from './edit-usercontact/edit-usercontact.component';
    import { AppRoutingModule } from './app.routing.module';
    import { ReactiveFormsModule } from '@angular/forms';

    @NgModule({
      declarations: [
        AppComponent,
        AddUsercontactComponent,
        UsercontactComponent,
        DashboardComponent,
        EditUsercontactComponent
      ],
      imports: [
        BrowserModule,
        ReactiveFormsModule,
        AppRoutingModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }

```    


**app.routing.module.ts**
    
``` typescript

    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router';
    import { AddUsercontactComponent } from './add-usercontact/add-usercontact.component';
    import { EditUsercontactComponent } from './edit-usercontact/edit-usercontact.component';
    import { DashboardComponent } from './dashboard/dashboard.component';

    const routes: Routes = [
      { path: 'edit', component: EditUsercontactComponent },
      { path: 'add', component: AddUsercontactComponent },
      { path: '', component: DashboardComponent }
    ];

    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule]
    })
    export class AppRoutingModule { }

```        


## Service in Angular 6 Application

Following is the implementation of our `UsercontactService`. It has all the API details that is required for the CRUD operation. The `UsercontactService` creates a JSON array with dummy user contacts, which can be modified but after restart the data will be reseted.

The CRUD functions: `create(), update(), getall(), delete()`

and some helper functions: `getUserById(), getnextId()`

**usercontact.service.ts**
    
``` typescript 

    import { Injectable } from '@angular/core';
    import { UserContact } from './usercontact.model';

    @Injectable({
      providedIn: 'root'
    })
    export class UsercontactService {

      usercontacts: UserContact[] = [{
        id: 0,
        firstname: 'Alex',
        lastname: 'BlaBla',
        email: 'alex.blabla@aol.at'
      },
      {
        id: 1,
        firstname: 'Otto',
        lastname: 'Blubb',
        email: 'otto.blubb@dsl.de'
      },
      {
        id: 2,
        firstname: 'Peter',
        lastname: 'Pan',
        email: 'peter.pan@neverland.com'
      }];

      create(usercontact: UserContact) {
        const itemIndex = this.usercontacts.length;
        usercontact.id = this.getnextId();
        console.log(usercontact);
        this.usercontacts[itemIndex] = usercontact;
      }

      delete(usercontact: UserContact) {
        this.usercontacts.splice(this.usercontacts.indexOf(usercontact), 1);
      }

      update(usercontact: UserContact) {
        const itemIndex = this.usercontacts.findIndex(item => item.id === usercontact.id);
        console.log(itemIndex);
        this.usercontacts[itemIndex] = usercontact;
      }

      getall(): UserContact[] {
        console.log('usercontactservice:getall');
        console.log(this.usercontacts);
        return this.usercontacts;
      }

      reorderUserContacts(usercontact: UserContact) {
        console.log('Zur Info:', usercontact);
        this.usercontacts = this.usercontacts
          .map(uc => uc.id === usercontact.id ? usercontact : uc)
          .sort((a, uc) => uc.id - uc.id);
      }

      getUserById(id: number) {
        console.log(id);
        const itemIndex = this.usercontacts.findIndex(item => item.id === id);
        console.log(itemIndex);
        return this.usercontacts[itemIndex];
      }

      getnextId(): number {
        let highest = 0;
        this.usercontacts.forEach(function (item) {
          if (highest === 0) {
            highest = item.id;
          }
          if (highest < item.id) {
            highest = item.id;
          }
        });
        return highest + 1;
      }
    }

```    
    

## Creating Components in Angular 6

After calling the URL the rquest is redirected to the `dashboard` and from there the user can perform crud operation.    
    

**dashboard.component.html**

Contains the button to create a new user contact and the placeholder of `usercontact.component`.
If the button create is clicked, the component `editUsercontact.component`, because the router calls the path `/add` which is defined in the 'app.routing.module.ts'.
    
``` html  

    <a [routerLink]="['/add']" class="btn btn-success ml-2">New</a>
    <hr>
    <app-usercontact></app-usercontact>

```    
    

**usercontact.component.html**

Pepares the table, which shows the user contacts from the JSON data.

``` html   

    <div class="usercontact-list">  
      <table class="table table-bordered">
      <thead>
        <tr>
          <th>Id</th>
          <th>Firstname</th>
          <th>Lastname</th>
          <th>Email</th>
          <th>Edit</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let usercon of usercontacts">
          <td>{{ usercon.id }}</td>
          <td>{{ usercon.firstname }}</td>
          <td>{{ usercon.lastname }}</td>
          <td>{{ usercon.email }}</td>
          <td>
            <button class=" btn btn-primary " (click)="editUserContact(usercon) ">Edit</button>
          </td>
          <td>
            <button class="btn btn-danger ml-2 " (click)="deleteUserContact(usercon) ">Delete</button>
          </td>
        </tr>
      </tbody>
      </table>
    </div>

```


**usercontact.component.ts**

Cotains all the functionality to show the JSON data and to handle all the action triggered by _delete_ and _edit_.

- ngOnInit(): provides the data for the table, by calling the `usercontact.service` with the function _getall()_
- editUserContact(): calls the router with path `edit` which redirects to `edit-usercontact.component`
- deleteUserContact(): calls the `usercontact.service` with the function _delete()_
   
``` typescript 

      import { Component, OnInit } from '@angular/core';
      import { UserContact } from '../share/usercontact.model';
      import { UsercontactService } from '../share/usercontact.service';
      import { Router } from '@angular/router';

      @Component({
        selector: 'app-usercontact',
        templateUrl: './usercontact.component.html',
        styleUrls: ['./usercontact.component.css'],
      })

      export class UsercontactComponent implements OnInit {

        usercontacts: UserContact[]; // Array<string>
        usercont: UserContact;

        constructor(private ucs: UsercontactService, private router: Router) {
        }

        editUserContact(usercontact: UserContact) {
          console.log(usercontact);
          localStorage.removeItem('editUserId');
          localStorage.setItem('editUserId', usercontact.id.toString());
          this.router.navigate(['edit']);
          // this.ucs.update(usercontact);
        }

        deleteUserContact(usercontact: UserContact) {
          console.log(usercontact);
          this.ucs.delete(usercontact);
        }

        ngOnInit() {
          console.log('usercontact:init');
          this.usercontacts = this.ucs.getall();
          console.log(this.usercontacts);
        }
      }

```    
    

**add-usercontact.component.html**

Shows the formular to create a new user contact. The Validators checking the input of the textfields.<br>
First name and last name will be checked if an input is done.<br>
The validator of Email field checks if an '@' is followed by any word character and a '.' with at least two any word character.

{% include lightbox.html src="/img/2018-07-26-Angular-6-CRUD-Example-Application/image003.jpg" title="VirtualBox" width="500" %}

``` html    

    <div class="col-md-6">
    <h2 class="text-center">Add User Contact</h2>
    <form [formGroup]="addForm" (ngSubmit)="onSubmit()">

      <div class="form-group">
        <label for="firstname">First Name:</label>
        <input formControlName="firstname" placeholder="First Name" name="firstname" class="form-control" id="firstname">
        <div class="feedback-red" *ngIf="isInvalid('firstname')">
          First Name is empty.
        </div>
      </div>

      <div class="form-group">
        <label for="lastname">Last Name:</label>
        <input formControlName="lastname" placeholder="Last name" name="lastname" class="form-control" id="lastname">
        <div class="feedback-red" *ngIf="isInvalid('lastname')">
          Last Name is empty.
        </div>
      </div>

      <div class="form-group">
        <label for="email">Email address:</label>
        <input type="email" formControlName="email" placeholder="Email" name="email" class="form-control" id="email">
        <div class="feedback-red" *ngIf="isEmailInvalid('email')">
          Email is not valid.
        </div>
      </div>

      <button button [disabled]="addForm.invalid" class="btn btn-success">Submit</button>
      <button class="btn btn-danger ml-2 " (click)="onCancel()">Cancel</button>
    </form>
  </div>

```    
    

**add-usercontact.component.ts**

Contains the functions:

- onSubmit(): Calls the `usercontact.service` the function _create_ and calls the router with the root entrie path.
- isInvalid(): Checks if the text field contains an entrie. If the entrie is valid the Submit-Button will be activated
- isEmailInvalid(): Checks if the text field contains an valid email.<br> With an '@' followed by a '.' for the domain. If the entrie is valid the Submit-Button will be activated.
    
``` typescript  

    import { Component, OnInit, EventEmitter, Output } from '@angular/core';
    import { UserContact } from '../share/usercontact.model';
    import { UsercontactService } from '../share/usercontact.service';
    import { FormBuilder, FormGroup, Validators } from '@angular/forms';
    import { first } from 'rxjs/operators';
    import { Router } from '@angular/router';

    @Component({
      selector: 'app-add-usercontact',
      templateUrl: './add-usercontact.component.html',
      styleUrls: ['./add-usercontact.component.css']
    })

    export class AddUsercontactComponent implements OnInit {

      constructor(private formBuilder: FormBuilder, private router: Router, private userService: UsercontactService) { }
      addForm: FormGroup;
      @Output()
      createUsercontact = new EventEmitter<UserContact>();

      emailRegex = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;

      ngOnInit() {
      this.addForm = this.formBuilder.group({
          id: [],
          email: ['', [Validators.required, Validators.pattern(this.emailRegex)]],
          firstname: ['', Validators.required],
          lastname: ['', Validators.required]
        });
      }

      isInvalid(name: string) {
        const control = this.addForm.get(name);
        return control.invalid && control.dirty;
      }

      isEmailInvalid(name: string) {
        const control = this.addForm.get(name);
        return control.invalid && control.dirty;
      }

      onSubmit() {
        this.userService.create(this.addForm.value);

        this.router.navigate(['']);
      }
    }

```    
    

**edit-usercontact.component.html**

Nearly the same as the `add-usercontact.component.html` but the text fields contains the values of the selected user contact.

{% include lightbox.html src="/img/2018-07-26-Angular-6-CRUD-Example-Application/image004.jpg" title="VirtualBox" width="500" %}
    
``` html  

    <div class="col-md-6">
      <h2 class="text-center">Change User Contact</h2>
      <form [formGroup]="addForm" (ngSubmit)="onSubmit()">

        <div class="form-group">
          <label for="firstname">First Name:</label>
          <input formControlName="firstname" placeholder="First Name" name="firstname" class="form-control" id="firstname">
          <div class="feedback-red" *ngIf="isInvalid('firstname')">
            First Name is empty.
          </div>
        </div>

        <div class="form-group">
          <label for="lastname">Last Name:</label>
          <input formControlName="lastname" placeholder="Last name" name="lastname" class="form-control" id="lastname">
          <div class="feedback-red" *ngIf="isInvalid('lastname')">
            Last Name is empty.
          </div>
        </div>

        <div class="form-group">
          <label for="email">Email address:</label>
          <input type="email" formControlName="email" placeholder="Email" name="email" class="form-control" id="email">
          <div class="feedback-red" *ngIf="isEmailInvalid('email')">
            Email is not valid.
          </div>
        </div>

        <button button [disabled]="addForm.invalid" class="btn btn-success">Submit</button>
        <button class="btn btn-danger ml-2 " (click)="onCancel()">Cancel</button>
      </form>
    </div>

``` 
    

**edit-usercontact.component.ts**
    
``` typescript 

    import { Component, OnInit, EventEmitter, Output } from '@angular/core';
    import { UserContact } from '../share/usercontact.model';
    import { UsercontactService } from '../share/usercontact.service';
    import { FormBuilder, FormGroup, Validators } from '@angular/forms';
    import { first } from 'rxjs/operators';
    import { Router } from '@angular/router';


    @Component({
      selector: 'app-edit-usercontact',
      templateUrl: './edit-usercontact.component.html',
      styleUrls: ['./edit-usercontact.component.css']
    })
    export class EditUsercontactComponent implements OnInit {

      constructor(private formBuilder: FormBuilder, private router: Router, private userService: UsercontactService) { }
      addForm: FormGroup;
      usercontact: UserContact;

      emailRegex = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;

      ngOnInit() {
        const userId = localStorage.getItem('editUserId');
        if (!userId) {
          alert('Invalid action.');
          this.router.navigate(['']);
          return;
        }
      this.addForm = this.formBuilder.group({
          id: [],
          email: ['', [Validators.required, Validators.pattern(this.emailRegex)]],
          firstname: ['', Validators.required],
          lastname: ['', Validators.required]
        });
        const data = this.userService.getUserById(+userId);
        this.addForm.setValue(data);
      }

        isInvalid(name: string) {
        const control = this.addForm.get(name);
        return control.invalid && control.dirty;
      }

      isEmailInvalid(name: string) {
        const control = this.addForm.get(name);
        return control.invalid && control.dirty;
      }

      onSubmit() {
        this.userService.update(this.addForm.value);
        this.router.navigate(['']);
      }

      onCancel() {
        this.router.navigate(['']);
      }
    }

```    
    

## Testing Angular 6 Application

Run the command `ng serve -o` the browser opens http://localhost:4200

After the browser opened, following screen shows the list of user contacts. On this page, avaiable actions to add, edit and delete user contacts. Following is a sample screen for add user contact.



## Conclusion

In this article, we learned about Angular 6 and created a sample example project using it. <br>
The source can be downloaded from github [here - Angular6-UserContact-App](https://github.com/wechris/Angular6-UserContact-App)

  
