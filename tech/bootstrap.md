To install **NG Bootstrap** in your Angular project, follow these steps:

### 1. Install NG Bootstrap
Run the following command in your Angular project directory:
```sh
npm install @ng-bootstrap/ng-bootstrap
```

### 2. Import `NgbModule`
Open your root module file (`app.module.ts`) or any feature module where you want to use NG Bootstrap components and import `NgbModule`:

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    NgbModule // Import NG Bootstrap module here
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

If you're using a **standalone component**, import `NgbModule` directly in `imports`:

```ts
import { Component } from '@angular/core';
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [NgbModule],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {}
```

### 3. Ensure Bootstrap CSS is Available
NG Bootstrap **does not include Bootstrap CSS**, so you need to install Bootstrap separately:
```sh
npm install bootstrap
```
Then, add it to your `angular.json`:
```json
"styles": [
  "node_modules/bootstrap/dist/css/bootstrap.min.css",
  "src/styles.css"
]
```

### 4. Use NG Bootstrap Components
Now you can use NG Bootstrap components in your templates. Example using an **accordion**:

```html
<ngb-accordion #acc="ngbAccordion" activeIds="ngb-panel-0">
  <ngb-panel title="First Panel">
    <ng-template ngbPanelContent>
      This is the content of the first panel.
    </ng-template>
  </ngb-panel>
  <ngb-panel title="Second Panel">
    <ng-template ngbPanelContent>
      This is the content of the second panel.
    </ng-template>
  </ngb-panel>
</ngb-accordion>
```

That's it! You now have NG Bootstrap installed and ready to use in your Angular project. 🚀