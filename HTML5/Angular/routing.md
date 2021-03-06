## 路由

### 基本應用

首頁的頁面

```ts
// src/app/home.component.ts
import { Component } from '@angular/core';

@Component({
  template: `
    <p>Home Page</p>
  `
})
export class HomeComponent { }
```

關於的頁面

```ts
// src/app/about.component.ts
import { Component } from '@angular/core';

@Component({
  template: `
    <p>About Page</p>
  `
})
export class AboutComponent { }
```

404 的頁面

```ts
// src/app/error.component.ts
import { Component } from '@angular/core';

@Component({
  template: `
    <p>Page not found</p>
  `
})
export class ErrorComponent { }
```

將製作好的頁面，放到 App 下的路由模組

```ts
// src/app/app-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { HomeComponent } from './home.component';
import { AboutComponent } from './about.component';
import { ErrorComponent } from './error.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: '**', component: ErrorComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

將製作好的頁面，宣告在 App 模組裡

```ts
// src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home.component';
import { AboutComponent } from './about.component';
import { ErrorComponent } from './error.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    AboutComponent,
    ErrorComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

再到 App 元件裡設定路由導覽

```ts
// src/app/app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h1>Angular GO</h1>
    <nav>
      <a routerLink="/home" routerLinkActive="active">Home</a>
      <a routerLink="/about" routerLinkActive="active">About</a>
    </nav>
    <router-outlet></router-outlet>
  `
})
export class AppComponent { }
```

增加一個書籍模組

```
// src/app/book/book.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HttpModule } from '@angular/http';

import { BookComponent } from './book.component';
import { BookService } from './book.service';

@NgModule({
  imports: [
    CommonModule,
    HttpModule
  ],
  declarations: [BookComponent],
  providers: [BookService]
})
export class BookModule {}
```

一個簡單伺服器

```js
// server.js
import express from 'express';

const app = express();

app.get('/book', (req, res) => {
  res.json([
    { id: 1, name: 'You Don\'t Know JS: Up & Going', author: 'Kyle Simpson' },
    { id: 2, name: 'You Don\'t Know JS: Scope & Closures', author: 'Kyle Simpson' },
    { id: 3, name: 'You Don\'t Know JS: this & Object Prototypes', author: 'Kyle Simpson' },
    { id: 4, name: 'You Don\'t Know JS: Types & Grammar', author: 'Kyle Simpson' },
    { id: 5, name: 'You Don\'t Know JS: Async & Performance', author: 'Kyle Simpson' },
    { id: 6, name: 'You Don\'t Know JS: ES6 & Beyond', author: 'Kyle Simpson' }
  ]);
});

app.listen(8000, () => {
  console.log('Server is running on http://localhost:8000');
});
```

### 巢狀路由
```ts
[...]

const routes: RouterConfig = [
  { path: '', redirectTo: 'home', terminal: true },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'about/:id', component: LinkComponent, children: [
    { path: '', redirectTo: 'overview', pathMatch: 'full' },
    { path: 'overview', component: OverviewComponent },
    { path: 'essential', component: EssentialComponent }
  ]}
];

[...]
```

### URL 參數
```ts
// src/app/app.routes.ts
import { provideRouter, RouterConfig } from '@angular/router';

import { HomeComponent } from './home.component';
import { AboutComponent } from './about.component';
import { LinkComponent } from './link.component';

const routes: RouterConfig = [
  { path: '', redirectTo: 'home', terminal: true },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'about/:id', component: LinkComponent }
];

export const APP_ROUTER_PROVIDERS = [
  provideRouter(routes)
];
```
```ts
//  src/app/about.component.ts
import { Component } from '@angular/core';
import { ROUTER_DIRECTIVES } from '@angular/router';

@Component({
  template: `
    <p>About Page</p>
    <ul *ngFor="let id of links">
      <li>
        <a [routerLink]="[id]">Link {{ id }}</a>
      </li>
    </ul>
  `,
  directives: [ROUTER_DIRECTIVES]
})
export class AboutComponent {
  public links: number[] = [1, 2, 3];
}
```
```ts
//  src/app/link.component.ts
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { Observable } from 'rxjs/Observable';

import 'rxjs/add/operator/map';

@Component({
  template: `
    <p>About Page - Link {{ id | async }}</p>
  `
})
export class LinkComponent {
  public id: Observable<string>;

  constructor(activatedRoute: ActivatedRoute) {
    this.id = activatedRoute
      .params
      .map(activatedRoute => activatedRoute.id);
  }
}
```

### 非同步路由

```js
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Routes, RouterModule } from '@angular/router';

export const ROUTES: Routes = [
  {
    path: '',
    component: FooComponent,
    children: [
      { path: 'bar', component: BarFooComponent },
      { path: 'baz', component: BazFooComponent }
    ]
  }
];

@NgModule({
  imports: [
    CommonModule,
    RouterModule.forChild(ROUTES)
  ],
  // ...
})
export class FooModule {}
```

```js
export const ROUTES: Routes = [
  {
    path: 'foo',
    loadChildren: './foo/foo.module#FooModule'
  }
];

@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot(ROUTES)
  ],
  // ...
})
export class AppModule {}
```
