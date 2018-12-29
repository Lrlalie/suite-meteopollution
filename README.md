
# suite-meteopollution

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
Now let's create a <strong>module</strong> meteo-pollution:
```
npm run ng generate module meteo-pollution
```
in meteo-pollution.module.ts : 
```
we will add an import : MeteoPollutionComponent
```
automatically the system generates an import at the top of the page:
```
import{MeteoPollutionComponent} from './meteo-pollution.component';
```
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
Let's create the <strong>sub-component</strong> meteo-pollution/city
```
npm run ng generate component meteo-pollution/city
```
let's create a shared <strong>module</strong> (where we could find necessary tools for modules and components)
```
npm run ng generate module shared (automatically : the system creates a shared.module.ts)
```
then in this module shared a <strong>sub-module</strong> material
```
npm run ng generate module shared/material (automatically : the system creates a material.module.ts)
```
In shared.module.ts : imports and exports : MaterialModule
```
imports:[CommonModule, MaterialModule],
exports:[MaterialModule],
```
automatically : the import is generated at the top of the page : import{MaterialModule} from './material/material.module';










