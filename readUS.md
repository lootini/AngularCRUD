Commands for an angular projects:

--we install nodejs--

mkdir Projects
sudo npm install -g @angular/cli

--Inside the Folder that we want to create the angular project--
ng new AngularCRUD
cd AngularCRUD

-- We install the json-server inside the project folder--
sudo npm install -g json-server

--We access the index.html of the project and we install bootstrap(jsDelivr)--

-- To build and launch the project --
ng serve -o

##HTML FORM in EMPLOYEE component.html ##

<nav class="navbar navbar-light bg-primary">
    <div class="container-fluid">
        <h1>Employe APP</h1>
        <div class="d-flex">
            <button type="button" data-bs-toggle="modal" data-bs-target="#exampleModal" class="btn btn-success">
                Add
            </button>
        </div>
    </div>
</nav>

<table class="table">
    <thead>
      <tr>
        <th scope="col">#ID</th>
        <th scope="col">First Name</th>
        <th scope="col">Last Name</th>
        <th scope="col">Email</th>
        <th scope="col">Phone</th>
        <th scope="col">Salary</th>
        <th scope="col">Action</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">1</th>
        <td>Mark</td>
        <td>Otto</td>
        <td>@mdo</td>
        <td>066121245</td>
        <td>10000</td>
        <td>
            <button class="btn btn-info">Edit</button>
            <button class="btn btn-danger mx-3">Delete</button>
        </td>
      </tr>
    </tbody>
  </table>

  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Employee Details</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
            <form [formGroup]="formValue">
                <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label">First Name</label>
                    <input type="text" formControlName="firstName" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                  </div>
                  <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label">Last Name</label>
                    <input type="text" formControlName="lastName" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                  </div>
                  <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label">Email address</label>
                    <input type="email" formControlName="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                  </div>
                  <div class="mb-3">
                    <label for="exampleInputEmail1" class="form-label">Phone</label>
                    <input type="text" formControlName="phone" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                  </div>
                <div class="mb-3">
                  <label for="exampleInputEmail1" class="form-label">Salary</label>
                  <input type="text" formControlName="salary" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                </div>
              </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary">Save</button>
        </div>
      </div>
    </div>
  </div>


-- Create a new component for our project --
ng g c EmployeeDashboard / g: generate, c:component

--Modal bootstrap--
https://getbootstrap.com/docs/5.0/components/modal/

--To get data from the form  code snippet in the employeecomponent.ts--
we need to import the form
import {FormBuilder, FormGroup} from '@angular/forms'
...

  formValue !: FormGroup;
  constructor(private formbuilder: FormBuilder) { }

  ngOnInit(): void {
    this.formValue = this.formbuilder.group({
      firstName : [''],
      lastName : [''],
      email : [''],
      phoneNumber : [''],
      salary : ['']
    })
  }

-- when creating the form module we need to import it in the app.module.ts--
ReactiveFormsModule

-- after creating the form we need to install the json-server--
sudo npm install -g json-server

--To run the json-server database--
json-server --watch db.json

-- now we gonna create services for CRUD inside a shared folder--
ng g s shared/api // g: generate s: service

-- inside shared/api.service.ts --
we gonna
import {HttpClient, HttpClientModule} from '@angular/common/http'

after that we need to add what we imported into app.module.ts in imports sections HttpClientModule

import {map} from 'rxjs/operators'

--Output of the POST GET UPDATE DELETE of the api service--

import { Injectable } from '@angular/core';
import {HttpClient, HttpClientModule} from '@angular/common/http'
import {map} from 'rxjs/operators'

@Injectable({
  providedIn: 'root'
})
export class ApiService {

  constructor(private http : HttpClient) { }

  postEmploye(data : any){
    return this.http.post<any>("http://localhost:3000/posts", data)
    .pipe(map((res:any)=>{
      return res;
    }))
  }

  getEmploye(){
    return this.http.get<any>("http://localhost:3000/posts")
    .pipe(map((res:any)=>{
      return res;
    }))
  }

  updateEmploye(data : any, id : number){
    return this.http.put<any>("http://localhost:3000/posts/" + id, data)
    .pipe(map((res:any)=>{
      return res;
    }))
  }

  deleteEmploye(id : number){
    return this.http.delete<any>("http://localhost:3000/posts/" + id)
    .pipe(map((res:any)=>{
      return res;
    }))
  }
}

-- After creating the class employee-dashboard.model.ts in the employee-dashboard anf filling the class with attributes : --
## using ng g m (name of the module)##

export class EmployeeModel{
    id : number = 0;
    firstName : string = '';
    lastName : string = '';
    email : string = '';
    phoneNumber : string = '';
    salary : string = '';
}

--we created an object of that class in the employee-dashboard.component.ts--
employeeModelObj : EmployeeModel = new EmployeeModel();

-- Now we create the methods for Post and Get and Update and Delete inside the employee-dashboard.component.ts

we import ApiService
import {ApiService} from '../shared/api.service'

and we create a constructor
private api : ApiService

and here is the code snippet of postEmployeDetails methode :

postEmployeeDetails(){
    this.employeeModelObj.firstName = this.formValue.value.firstName;
    this.employeeModelObj.lastName = this.formValue.value.lastName;
    this.employeeModelObj.email = this.formValue.value.email;
    this.employeeModelObj.phoneNumber = this.formValue.value.phoneNumber;
    this.employeeModelObj.salary = this.formValue.value.salary;

    this.api.postEmploye(this.employeeModelObj)
    .subscribe(res => {
      console.log(res);
      alert("Employee added Successfully")
    },
    err => {
      alert("Something went wrong")
    })
  }


  we link this method to the add button in the html employee-dashboard.component.html with click event :
  <button type="button" (click)="postEmployeeDetails()" class="btn btn-primary">Add</button>

  --After the employee insert his details info the info should be reset by : --
  under the alert("Employee added Successfully")
  this.formValue.reset();

  --We want to cancel the form after the employee clicked the add button we will do that by giving to the cancel button an id="cancel" and we will come back to employee component.ts and make a ref variable under the alert("Employee added Successfully")

  let ref = document.getElementById('cancel')
  ref?.click();

  --We created getAllEmployee()-- in employee component
  getAllEmployee(){
    this.api.getEmploye()
    .subscribe(res => {
      this.employeeData = res;
    })
  }

  --We deleted the static data that we inserted in our html form and we replaced it with --

   <tbody>
        <tr *ngFor="let row of employeeData">
            <td>{{row.id}}</td>
            <td>{{row.firstName}}</td>
            <td>{{row.lastName}}</td>
            <td>{{row.email}}</td>
            <td>{{row.phoneNumber}}</td>
            <td>{{row.salary}}</td>
            <td>
                <button class="btn btn-info">Edit</button>
                <button class="btn btn-danger mx-3">Delete</button>
            </td>
        </tr>
    </tbody>

  $$ We declare the getAllEmployee method in the ngOnInit methode and postEmployeDetails
  this.getAllEmployee();

    -- Now we create the delete method in the component of our employee dashboard
 deleteEmployee(row : any){
    this.api.deleteEmploye(row.id)
    .subscribe(res => {
      alert("Employee Deleted");
      this.getAllEmployee();
    })
  }
    -- We call the delete method in the html form using the onclick
    <button (click)="deleteEmployee(row)" class="btn btn-danger mx-3">Delete</button>
    we passed the row into our method so that we can acess the rows in the component employee dashboard to modify and access a specefic row like in this case we will access ID cuz we gonna the delete methode require the ID

    ## !!! In the delete and update methods of the ApiService make sure that http://localhost:3000/posts/ (finishes with /) !!!!

    -- In this step we gonna make the edit button the purpose is that the edit button need to open the form with the actual data --

    ## we edited the EDIT button in the html so that we can access the form

  <button (click)="onEdit(row)" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal" class="btn btn-info">Edit</button>

    ## but the next step is to edit the actual data of the form

  -- we add those variable before constructor and Create the onEdit method in the employee component after deleteEmploye
  showAdd !: boolean;
  showUpdate !: boolean;

  ##Method##
onEdit(row : any){
    this.showAdd = false;
    this.showUpdate = true;
    this.employeeModelObj.id = row.id;
    this.formValue.controls['firstName'].setValue(row.firstName)
    this.formValue.controls['lastName'].setValue(row.lastName)
    this.formValue.controls['email'].setValue(row.email)
    this.formValue.controls['phoneNumber'].setValue(row.phoneNumber)
    this.formValue.controls['salary'].setValue(row.salary)
  }


  -- Now we gonna create a new button under the SAVE BUTTON to update the actual form and and new method to update --
  <button type="button" (click)="updateEmployeeDetails()" class="btn btn-primary">Update</button>
  ##Method##
  updateEmployeeDetails(){
    this.employeeModelObj.firstName = this.formValue.value.firstName;
    this.employeeModelObj.lastName = this.formValue.value.lastName;
    this.employeeModelObj.email = this.formValue.value.email;
    this.employeeModelObj.phoneNumber = this.formValue.value.phoneNumber;
    this.employeeModelObj.salary = this.formValue.value.salary;
    this.api.updateEmploye(this.employeeModelObj, this.employeeModelObj.id)
    .subscribe(res => {
      alert("Updated Successfully")
      let ref = document.getElementById('cancel')
      ref?.click();// to click (start) the cancel button to clear the screen from the form
      this.formValue.reset(); //to reset the form so that we can have a clean form
      this.getAllEmployee(); // recalls the method from the start
    })
  }

-- Now we need to hide the add button when we click the edit button we should only see the update button to update the form and same goes for the add employee button it should only show a blank form with no update button --
## so we create two boolean variables and a function named clickAddEmployee
## var ##
    showAdd !: boolean;
    showUpdate !: boolean;

##Method clickAddEmployee ##
clickAddEmployee(){
    this.formValue.reset();     //This one reset the form to a blank one with no input
    this.showAdd = true;        //We added it to the onEdit too function as fasle
    this.showUpdate = false;    //We added it to the onEdit too function as true
  }


--Now we update the 'Add Employee' button to insert the clickAddEmployee on click event
<button (click)="clickAddEmployee()" type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#exampleModal">Add Employee</button>

--For the Add button we added *ngIf="showAdd" in the attributes
<button type="button" *ngIf="showAdd" (click)="postEmployeeDetails()" class="btn btn-primary">Add</button>

--For the Update button we added *ngIf="showUpdate" in the attributes
<button type="button" *ngIf="showUpdate" (click)="updateEmployeeDetails()" class="btn btn-primary">Update</button>




