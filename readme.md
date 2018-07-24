This readme describes how to build a minimal Angular app from scratch

We will use SystemJS instead of Webpack to load ESM

Call npm init -y to get minimum package.json file.

We need the following third-party dependencies.
- core-js
- rxjs
- zone.js

npm i --save core.js zone.js rxjs
npm i --save systemjs

Create systemjs.config.js configuration file.
Copy following content:

Start of systemjs.config.js

System.config({
    paths: {
      'npm:': '/node_modules/'
    },
    map: {
      app: 'dist/app',
      '@angular/core': 'npm:@angular/core/bundles/core.umd.js',
      '@angular/common': 'npm:@angular/common/bundles/common.umd.js',
      '@angular/compiler': 'npm:@angular/compiler/bundles/compiler.umd.js',
      '@angular/platform-browser': 'npm:@angular/platform-browser/bundles/platform-browser.umd.js',
      '@angular/platform-browser-dynamic': 'npm:@angular/platform-browser-dynamic/bundles/platform-browser-dynamic.umd.js',
      'core-js': 'npm:core-js',
      'zone.js': 'npm:zone.js',
      'rxjs': 'npm:rxjs',
      'rxjs-compat': 'npm:rxjs-compat',
      'tslib': 'npm:tslib/tslib.js'
    },
    packages: {
      'dist/app': {},
      'rxjs': {},
      'rxjs-compat': {},
      'core-js': {},
      'zone.js': {}
    }
  });

// End of systemjs.config.js

Install Angular core dependencies
npm i --save @angular/core @angular/compiler @angular/common @angular/platform-browser @angular/platform-browser-dynamic

Now we need to setup TypeScript

npm i --save-dev typescript

Create tsconfig.json and copy the following content:

{
    "compilerOptions": {
      "outDir": "dist",
      "module": "commonjs",
      "moduleResolution": "node",
      "experimentalDecorators": true,
      "emitDecoratorMetadata": true,
      "lib": [
        "dom",
        "es2015"
      ]
    }
  }

Add the following script to package.json

"scripts": {
    "build": "tsc"
},

Create a file called index.html with the following content:
<html>
  <head>
    <title>Hello, Angular</title>
  </head>
  <body>
    <app-main>Loading...</app-main>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      System.import('dist/main.js').catch(function (err) {
          console.error(err);
      });
    </script>
  </body>
</html>

Create the /dist folder.
Create a src/app folder.
Create the app.component.ts file into src/app.
Add the basic app.component structure.

import { Component } from '@angular/core';

@Component({
    selector: 'app-main',
    template: '<h1>Hello, {{name}}</h1>'
})
export class AppComponent {
    name: 'Angular';
}

Create the main module of our application app.module.ts file into src/app.

import { AppComponent } from './app.component';
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
    imports: [BrowserModule],
    declarations: [AppComponent],
    bootstrap: [AppComponent]
})
export class AppModule {    
}

Create the main.ts file at the root of the src folder.
Add the following content:

import 'core-js/es7/reflect';
import 'zone.js/dist/zone';
import { platformBrowserDynamic } 
                     from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
platformBrowserDynamic().bootstrapModule(AppModule);

Now compile it with npm run build

Install a simple http server called live-server.
npm i --save-dev live-server

Update the package.json file to add the server.
"scripts": {
  "build": "tsc",
  "start": "live-server"
},

Run it with npm start.

