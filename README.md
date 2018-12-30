
# setting-up-Angular7-meteopollution the following

beforehand begin the angular's installation : create a folder with a neutral name (for example : MyAngularFolder), and open the gitbash console 
from this folder to install angular/cli.

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
- in index.html 
```
<body><mp-root></mp-root></body>
```
- and in AppComponent.ts
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
we will find this new sub-component under app
in meteo-pollution component:

in meteo-pollution component.ts:
```
selector: 'mp-meteo-pollution',
```
in meteo-pollution component.html:
```
delete all and write : 
<mp-meteo-pollution></mp-meteo-pollution>
```
### module meteo-pollution
Now let's create a <strong>module</strong> meteo-pollution:
```
npm run ng generate module meteo-pollution
```
in meteo-pollution.module.ts : 
```
we will add a declarations : [MeteoPollutionComponent] (not in the imports)
```
automatically the system generates an import at the top of the page:
```
import{MeteoPollutionComponent} from './meteo-pollution.component';
```
### app.module
in app.module.ts:
```
delete the declaration: MeteoPollutionComponent
```
I suppose that its place is in the meteo-pollution.module.ts, and it can be declared only once, then
```
add MeteoPollutionModule in the imports
```
The import{MeteoPollutionModule} from './meteo-pollution/meteo-pollution-module';
It is automatically generated at the top of the page.

If this component should be used outside its module, it should be exported (else : nullPointerException), so 
in meteo-pollution.module.ts
```
exports:[MeteoPollutionComponent],
```
### sub-component city (path : app/meteo-pollution)
Let's create the <strong>sub-component</strong> meteo-pollution/city
```
npm run ng generate component meteo-pollution/city
```

### sub-component city 
and Let's create the <strong>sub-component</strong> meteo-pollution/cities
```
npm run ng generate component meteo-pollution/cities
```

#### in meteo-pollution.module.ts add:
```
(automatically the imports are generated at the top of the page)
import{MeteoPollutionComponent} from './meteo-pollution.component';
imports{CityComponent} from './city/city.component';
declarations:[MeteoPollutionComponent,CityComponent]
in imports:[CommonModule, SharedModule], exports:[MeteoPollutionComponent],
```
### module shared
let's create a shared <strong>module</strong> (where we could find necessary tools for modules and components)
```
npm run ng generate module shared (automatically : the system creates a shared.module.ts)
```
### sub-module material (in shared module)
then in this module shared a <strong>sub-module</strong> material
```
npm run ng generate module shared/material (automatically : the system creates a material.module.ts)
```
In shared.module.ts : imports and exports : MaterialModule
```
imports:[CommonModule, MaterialModule,FlexLayoutModule],
exports:[MaterialModule, FlexLayoutModule ],
```
automatically : the import is generated at the top of the page : import{MaterialModule} from './material/material.module';

In meteo-pollution.module.ts add the import of SharedModule
```
declarations:[MeteoPollutionComonent], imports:[CommonModule,SharedModule], exports:[MeteoPollutionComponent]
```
## OTHER COMPONENTS
### CHOOSE YOUR BUTTON
https://material.angular.io/components/categories :on this site, get the reference of the API.
to find the link : in the left menu, clic on
```
button
```
then clic on 
```
api's tab
``` 
(This link will create a button) copy the following link in material.module.ts (in imports and exports)
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
```
<button mat-icon-button>
<mat-icon>menu</mat-icon>
</button>
```
copy-paste this code in meteo-pollution.component.html 

### Models and city.model.ts
in Shared (path : app/shared)
Handly, create a folder names Models, and in Models, create a file names city.model.ts
in cities.model.ts
```
export class City{
public name:string;
public position: number;
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
#### in city.component.ts : add
under export class ...
```
public city: string;
constructor(){
this.city="Lyon"; //ici cela nous servira de test à corriger par la suite
}
ngOnInit(){
<mat-icon aria-label="loc_off">location_off</mat-icon>
<span>{{city}}</span> //double {{ ou ${
}
```
### HTTPREQUEST : création d'un service (path : meteo-pollution/shared/services/location-iq)
Pour pouvoir réutiliser cette fonction dans plusieurs modules c'est poruquoi on crèe un service
```
npm run ng generate service meteo-pollution/shared/services/location-iq
```
#### in city.component.ts : add
in export class : 
```
export class CityComponent implements OnInit{
@Input() city:City; // the city's name becomes dynamic
constructor(private LocationIQService : LocationIqService){
this.FindLocation();
}

//ne pas modifier la suite pour le moment
ngOnInit(){
<mat-icon aria-label="loc_off">location_off</mat-icon>
<span>{{city}}</span> //double {{ ou ${
}
```
#### in shared.module.ts add import of HttpClientModule 
```
import {httpClientModule} from '@angular/common/http';
declarations: [],
imports: [CommonModule,MaterialModule,HttpClientModule,FlexLayoutModule],
exports [MaterialModule,HttpClientModule,FlexLayoutModule],
```
#### in city.component.html
```
<mat-icon *ngIf ="!city || !city.position; else search">location_off</mat-icon> //Since the end of Angular4 :using else
<ng-template #search>
</mat-icon>location_on</mat-icon>
</ng-template>
```
Here, if there is no city, the icon will be : location_off, else location_on.
it is the geolocation that allows to locate the city

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
#### Environment.ts
(path : src/environments/environment.ts)
add (under text automatically generated)
```
export const locationIQ ={
key:----write your key----
};
```
#### in location-iq-service.ts, add:
```
import {HttpClient} from '@angular/common/http';
import{locationIQ} from './../../../../environments/environment';
...
constructor(private http:HttpClient){ 
get(position:Position):Observable<any>{
return
this.http.get('https://eu1.locationiq.com/v1/reverse.php,key=${locationIQ.key}&lat=${position.coords.latitude}&lon=${position.coords.longitude}&format=json');
}
}
  ```

#### in location-iq-models.ts
```
import{Address} from './address.model';
export class LocationIQ{
address:Address;
}
```
#### in shared /models
Right clic/new file/location-iq-model.ts
Create an export class LocationIQ

#### in shared/models
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

#### in location-iq-service.ts, add:
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
import{LocationIQ} from ‘src/environments/environment’;

@Component({
Selector :’mp-city’,
templateUrl :’./city.component.html’
styleUrls: [‘./city.component.scss]
})
export class CityComponent{
@Input()city : City;
localizeMe=false;
@output() onCity:EventEmitter<City>;

constructor(private locationIQService:LocationIqService, private snack:MatSnackBar){
this.findLocation();
this.onCity = new EventEmitter;
}
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
findCityName():Subscription{
return this.locationIQService.get(this.city.position).subscribe((locationIQ: LocationIQ)=>{
this.city.address = locationIQ.address;
this.onCity.emit(this.city);
},
(error:HttpErrorResponse)=> this.snack.open(
“City location Error”,
“Retry”).onAction().subscribe(() =>this.findCityName())
}
}

```

 









