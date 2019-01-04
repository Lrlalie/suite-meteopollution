
# setting-up-Angular7-meteopollution the following

beforehand begin the angular's installation : create a folder with a neutral name (for example : MyAngularFolder), and open the gitbash console (for instance : in desktop folder)
from this folder to install angular/cli.

On Browsers : localhost:4200/

## begin installation with Cyril's README

1) ENOENT ERROR : with Cyril we noticed that it could appear an error with npm : ENOENT ERROR : package.json missing: 

```
npm init 
```
the system ask you question :
```
click on the "enter" button
```
until it creates a json model without necessarily completing it
you will see appear in your home folder (we called it: myAngularFolder): your package json.
and you can install angular/cli : npm install @angular/cli

2) tslint Missing : Another note, sometimes the tslint can also be missing to fix it:
```
npm install tslint
```
## After angular was installed :
begin Application in VSCode
we added a prefix to our application meteopollution : mp
verify if mp-root is:
### in index.html 
```
<body><mp-root></mp-root></body>
```
### in AppComponent.ts
```
selector: 'mp-root',
``` 
## THE COMPONENTS AND MODULES
### sub-component meteo-pollution
Our AppComponent represents our total Application, we're going to create <strong>sub-components</strong> :
first sub-component (in VSCODE console):
```
npm run ng generate component meteo-pollution
```

### in meteo-pollution component (path :app/meteo-pollution.component)
#### in meteo-pollution component.ts:
```
selector: 'mp-meteo-pollution',
```
#### in meteo-pollution component.html:
```
delete all and write : 
<mp-meteo-pollution></mp-meteo-pollution>
```
### Creation: module meteo-pollution
```
npm run ng generate module meteo-pollution
```
### in meteo-pollution.module.ts : add
```
declarations : [MeteoPollutionComponent] (not in the imports)
import{MeteoPollutionComponent} from './meteo-pollution.component';(automatically generated)
```
### app.module
in app.module.ts:
```
delete the declaration: MeteoPollutionComponent
```
I suppose that its place is in the meteo-pollution.module.ts, and it can be declared only once, then
```
The import{MeteoPollutionModule} from './meteo-pollution/meteo-pollution-module';
It is automatically generated at the top of the page.
add MeteoPollutionModule in the imports
```

If this component should be used outside its module, it should be exported (else : nullPointerException), so

### in meteo-pollution.module.ts
```
exports:[MeteoPollutionComponent],
```
### display our application
#### in app.component.html
```
<mp-meteo-pollution> </mp-meteo-pollution> 
```
### sub-component city (path : app/meteo-pollution)
Let's create the <strong>sub-component</strong> meteo-pollution/city
```
npm run ng generate component meteo-pollution/city
```

### sub-component cities
and Let's create the <strong>sub-component</strong> meteo-pollution/cities
```
npm run ng generate component meteo-pollution/cities
```
### in meteo-pollution.module.ts add:
```
import{MeteoPollutionComponent} from './meteo-pollution.component';(automatically generated)
imports{CityComponent} from './city/city.component';(automatically generated)
imports{CitiesComponent} from './cities/cities.component';(automatically generated)
declarations:[MeteoPollutionComponent,CityComponent,CitiesComponent]
in imports:[CommonModule, SharedModule], exports:[MeteoPollutionComponent],
```
### Creation module shared
let's create a shared <strong>module</strong> (where we could find necessary tools for modules and components)
```
npm run ng generate module shared (automatically : the system creates a shared.module.ts)
```
#### in shared.module.ts : create a sub-module material 
then in this module shared a <strong>sub-module</strong> material
```
npm run ng generate module shared/material (automatically : the system creates a material.module.ts)
```
#### In shared.module.ts : add
```
imports:[CommonModule, MaterialModule,FlexLayoutModule],
exports:[MaterialModule, FlexLayoutModule],
import{MaterialModule} from './material/material.module';(automatically generated)
```
#### in material.module.ts add
```
imports:[CommonModule,MatButtonModule,MatToolBarModule,MatIconModule],
exports:[MatButtonModule,MatToolBarModule,MatIconModule]
import{CommonModule} from '@angular/common';(automatically generated)
import{MatButtonModule} from '@angular/material/button';(automatically generated)
import{MatToolBarModule} from '@angular/materail/toolbar';(automatically generated)
import{MatIconModule} from '@angular/material/icon';(automatically generated)
```

## OTHER COMPONENTS
### CHOOSE YOUR BUTTON more details on icons, buttons
https://material.angular.io/components/categories :on this site, get the reference of the API.
to find the link : in the left menu, clic on
```
button
```
then clic on 
```
api's tab
```
#### in material.module.ts
(This link will create a button) copy the following link (in imports and exports)
```
import {MatButtonModule} from '@angular/material/button';
imports:[CommonModule, MatButtonModule],exports:[MatButtonModule]
```
In the web site, clic on 
```
the examples' tab
```
### CHOOSE YOUR ICONS
https://material.io/tools/icons/?style=baseline
Let's navigate in the icons and get the name of the one that interests you: ex: "menu" and "location_on" and its contrary "location_off"
and select a form to your button, for example : <strong>icon</strong>. Then clic on 
```
<>
```
you will reach codes, search for the Icon Buttons and select a part of code, for instance : 
copy-paste this code
#### in meteo-pollution.component.html 
```
<button mat-icon-button>
<mat-icon>menu</mat-icon>
</button>
```
### in shared : creation folder : Models and in Models create file :city.model.ts
```
export class City{
public name:string;
public position: Position;
}
```
#### in meteo-pollution.component.ts: add
```
import{CityComponent} from './city/city.component';
import{City} from '../shared/models/city.model';

export class MeteoPollutionComponent implements OnInit{
public city:City;
constructor(){
this.city = new City();
}
```
#### in meteo-pollution.component.html: add
```
<mat-toolbar color ="primary">
<mat-toolbar-row>
<button mat-icon-button>
<mat-icon aria-label="menu">menu</mat-icon> //aria label non impératif
</button>
<mp-city[city] = "city"></mp-city>
</mat-toolbar-row>
</mat-toolbar>
```
### HTTPREQUEST : création d'un service (path : meteo-pollution/shared/services/location-iq)
Pour pouvoir réutiliser cette fonction dans plusieurs modules c'est poruquoi on crèe un service
```
npm run ng generate service meteo-pollution/shared/services/location-iq
```
#### in city.component.html
```
<mat-icon *ngIf ="!city || !city.position; else search">location_off</mat-icon> //Since the end of Angular4 :using else
<ng-template #search>
</mat-icon>location_on</mat-icon>
</ng-template>
```
Here, The geolocation locates the city, if there is no city, the icon will be : location_off, else location_on.

#### in city.component.ts : add
```
import{Component, OnInit, Input} from '@angular/core';
import{City} from 'src/app/shared/models/city.model';
import{LocationIqService} from '../shared/services/location-iq.service';
import{MatSnackBar} from '@angular/material/snack-bar';
...
export class CityComponent implements OnInit{
@Input()city : City;
localizeMe=false;
...
constructor(private locationIQService:LocationIqService, private snack:MatSnackBar){
this.findLocation();
}
findLocation(){
navigator.geolocation.getCurrentPosition(
(event:position)=>{
this.city.position=event;
this.findCityName(event);
},
(event:PositionError)=>this.snack.open(
"Geolocation Error",
"Retry").onAction().subscribe(()=>this.findLocation())
);
}
findCityName(event.Position){
this.localizeMe=true;
this.city.name=reponse['address']['city'];},);
}
}
```
#### Environment.ts (path : src/environments/environment.ts)
under text automatically generated, add
```
export const locationIQ ={
key:----write your key----
};
```
The key should be obtained in the locationIQ site.

#### in shared /models : create location-iq-model.ts
Right clic/new file/location-iq-model.ts
In this model, create an export class LocationIQ

#### in location-iq-models.ts
```
import{Address} from './address.model';
export class LocationIQ{
address:Address;
}
```
#### in location-iq-service.ts
let's create the location-iq-service.ts : 
```
npm run ng generate service meteo-pollution/shared/services/location-iq
```
```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { locationIQ } from './../../../../environments/environment';
import { Observable } from 'rxjs';
import { LocationIQ } from 'src/app/shared/models/location-iq.models';

@Injectable({
  providedIn: 'root'
})
export class LocationIqService {

  constructor(private http: HttpClient) { }

    get(position:Position): Observable<LocationIQ>{
      return this.http.get<LocationIQ>(`https://eu1.locationiq.com/v1/reverse.php?key=${
        locationIQ.key
      }&lat=${
        position.coords.latitude
      }&lon=${
        position.coords.longitude
      }&format=json`);
    }
  
}
```

#### in shared/models : address.model.ts
Right clic/new file/address.model.ts
Export class Address{
County:string;
State:string;
Postcode:string;
Country:string;
}

origin of this class: on the https://locationiq.com site, go down to the bottom of the first page, select the reverse insert, click on lookup, then on json output. Data is obtained in json format. We choose address and we copy the content.

"address": {
        "house_number": "362",
        "road": "5th Avenue",
        "suburb": "Midtown South",
        "city_district": "Manhattan",
        "city": "New York City",
        "county": "New York County",
        "state": "New York",
        "postcode": "10001",
        "country": "USA",
        "country_code": "us"
    },	
We obtain json data, which are going to be simplified and typed in string (because all data in json are embed with double quotes, so it is string.
We keep the data that interests us : City, county, country and postcode.

##### in city.model.ts 
```
import{Address} from './address.model'; //there is a cluster mention ???
export class City{
position: Position;
address: Address; 
}

```
#### in shared/models : let's create a file : address.model.ts
Right clic/new file/address.model.ts
```
Export class Address{
County:string;
State:string;
Postcode:string;
Country:string;
}
```
origin of this class: on the https://locationiq.com site, go down to the bottom of the first page, select the reverse insert, click on lookup, then on json output. Data is obtained in json format. We choose address and we copy the content.

"address": {
        "house_number": "362",
        "road": "5th Avenue",
        "suburb": "Midtown South",
        "city_district": "Manhattan",
        "city": "New York City",
        "county": "New York County",
        "state": "New York",
        "postcode": "10001",
        "country": "USA",
        "country_code": "us"
    },	
We obtain json data, which are going to be simplified and typed in string (because all data in json are embed with double quotes, so it is string.
We keep the data that interests us : City, county, country and postcode.

#### in shared/services/location-iq-service.ts, change the observable's type:
```
…
get(position:Position):Observable<LocationIQ>{
…
```
All service’s method return an “Observable”. 
Other example : getCurrentPosition

#### in meteo-pollution.component.html
```
<mat-toolbar color ="primary">
<mat-toolbar-row>
<button mat-icon-button>
<mat-icon aria-label="menu">menu</mat-icon> //aria label non impératif
</button>
<mp-city (onCity)=”addCity($event)”[city] = "city"></mp-city>
</mat-toolbar-row>
</mat-toolbar>
```
#### in City.component.ts
```
import{Component, OnInit, Input, Output, EventEmitter} from '@angular/core';
import{City} from 'src/app/shared/models/city.model';
import{LocationIqService} from '../shared/services/location-iq.service';
import{MatSnackBar} from '@angular/material/snack-bar';
import{Subscription} from 'rxjs';
import{LocationIQ} from ‘src/app/shared/models/location-iq.model’;
import{HttpErrorResponse} from ‘@angular/common/http';
import{locationIQ} from ‘src/environments/environment’;//this is the key

@Component({
Selector :'mp-city',
templateUrl :'./city.component.html'
styleUrls: ['./city.component.scss']
})
export class CityComponent{
@Input()city : City; //kind of inheritance parent =>child
localizeMe=false;
@output() onCity:EventEmitter<City>;

constructor(private locationIQService:LocationIqService, private snack:MatSnackBar){
this.findLocation();
this.onCity = new EventEmitter;
}

//creation function called above
findLocation(){
navigator.geolocation.getCurrentPosition(
(event:position)=>{
this.city.position=event;
this.findCityName();
},
(event:PositionError)=>this.snack.open(
"Geolocation Error",
"Retry").onAction().subscribe(()=>this.findLocation())
);
}

//creation function called above
findCityName():Subscription{
return this.locationIQService.get(this.city.position).subscribe((locationIQ : LocationIQ)=>{
this.city.address = locationIQ.address;
this.onCity.emit(this.city);
},
(error:HttpErrorResponse)=> this.snack.open(
“City location Error”,
“Retry”).onAction().subscribe(() =>this.findCityName())
);
}
}

```
#### in shared.module.ts add import of HttpClientModule 
```
import {httpClientModule} from '@angular/common/http';
declarations: [],
imports: [CommonModule,MaterialModule,HttpClientModule,FlexLayoutModule],
exports [MaterialModule,HttpClientModule,FlexLayoutModule],
```
#### meteo-pollution.component.ts
```
import {Component} from '@angular/core';
import {CityComponent} from './city/city.component';
import {City} from '../shared/models/city.model';
import {log} from 'util';

@Component({
selector:'mp-meteo-pollution',
templateUrl: './meteo-pollution.component.html';
styleUrls:['./meteo-pollution.component.scss']
})

export class MeteoPollutionComponent{
public city:city;
constructor(){
this.city = new city;
}
addCity(city:City){
console.log("wait");
}
}
```
## let's create the meteo and pollution components : 
### in app.component.html
#### sub-component meteo (path : app/meteo-pollution)
Let's create the <strong>sub-component</strong> meteo-pollution/meteo
```
npm run ng generate component meteo-pollution/meteo
```
#### sub-component pollution (path : app/meteo-pollution)
Let's create the <strong>sub-component</strong> meteo-pollution/pollution
```
npm run ng generate component meteo-pollution/pollution
```







