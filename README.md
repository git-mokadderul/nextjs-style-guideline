# **🚀 Next.js Style Guide**

## Table of Contents

### **Single responsibility**

1. [Rule of One](#Rule-of-One)

### **Naming**

2. [General Naming Guidelines](#General-Naming-Guidelines)
3. [Separate file names with dots and dashes](#Separate-file-names-with-dots-and-dashes)
4. [Symbols and file names](#Symbols-and-file-names)
5. [Service names](#Service-names)
6. [Bootstrapping](#Bootstrapping)
7. [Component custom prefix](#Component-custom-prefix)
8. [Directive selectors](#Directive-selectors)
9. [Directive custom prefix](#Directive-custom-prefix)
10. [Pipe names](#Pipe-names)
11. [Unit test file names](#Unit-test-file-names)

### **Performance and Data Handling**

7. [Caching Data in Next.js](#caching-data-in-nextjs)
8. [Use Route Handlers](#use-route-handlers)
9. [Use Multiple Data Rendering Modes](#use-multiple-data-rendering-modes)

### **Advanced Features**

10. [TypeScript Support](#typescript-support)
11. [Enhance SEO in Next.js](#enhance-seo-in-nextjs)

### **Deployment**

12. [Deploy Your Next.js App to Vercel](#deploy-your-nextjs-app-to-vercel)

### **Best Practices**

13. [Client Component Placement Strategy in Next.js](#client-component-placement-strategy-in-nextjs)

---

# File structure conventions

Some code examples display a file that has one or more similarly named companion files. For example, hero.component.ts and hero.component.html.

The guideline uses the shortcut hero.component.ts|html|css|spec to represent those various files. Using this shortcut makes this guide's file structures easier to read and more terse.

# Single responsibility

Apply the single responsibility principle (SRP) to all components, services, and other symbols. This helps make the application cleaner, easier to read and maintain, and more testable.

## Rule of One

Style 01-01
Do define one thing, such as a service or component, per file.

Consider limiting files to 400 lines of code.

Why?
One component per file makes it far easier to read, maintain, and avoid collisions with teams in source control.

Why?
One component per file avoids hidden bugs that often arise when combining components in a file where they may share variables, create unwanted closures, or unwanted coupling with dependencies.

Why?
A single component can be the default export for its file which facilitates lazy loading with the router.

The key is to make the code more reusable, easier to read, and less mistake-prone.

The following negative example defines the AppComponent, bootstraps the app, defines the Hero model object, and loads heroes from the server all in the same file. Don't do this.

1. app/heroes/hero.component.ts:

   ```/* avoid */
   import {Component, NgModule, OnInit} from '@angular/core';
   import {BrowserModule} from '@angular/platform-browser';
   import {platformBrowserDynamic} from '@angular/platform-browser-dynamic';
   interface Hero {
   id: number;
   name: string;
   }
   @Component({
   selector: 'app-root',
   template: `
      <h1>{{title}}</h1>
      <pre>{{heroes | json}}</pre>
    `,
   styleUrls: ['../app.component.css'],
   standalone: false,
   })
   export class AppComponent implements OnInit {
   title = 'Tour of Heroes';
   heroes: Hero[] = [];
   ngOnInit() {
    getHeroes().then((heroes) => (this.heroes = heroes));
   }
   }
   @NgModule({
   imports: [BrowserModule],
   declarations: [AppComponent],
   exports: [AppComponent],
   bootstrap: [AppComponent],
   })
   export class AppModule {}
   platformBrowserDynamic().bootstrapModule(AppModule);
   const HEROES: Hero[] = [
   {id: 1, name: 'Bombasto'},
   {id: 2, name: 'Tornado'},
   {id: 3, name: 'Magneta'},
   ];
   function getHeroes(): Promise<Hero[]> {
   return Promise.resolve(HEROES); // TODO: get hero data from the server;
   }
   ```

It is a better practice to redistribute the component and its supporting classes into their own, dedicated files.

2. main.ts:

   ```import {platformBrowserDynamic} from '@angular/platform-browser-dynamic';
   import {AppModule} from './app/app.module';
   platformBrowserDynamic().bootstrapModule(AppModule);
   ```

As the application grows, this rule becomes even more important.
**[⬆ back to top](#table-of-contents)**

# Naming

Naming conventions are hugely important to maintainability and readability. This guide recommends naming conventions for the file name and the symbol name.

## General Naming Guidelines

Style 02-01
Do use consistent names for all symbols.

Do follow a pattern that describes the symbol's feature then its type. The recommended pattern is feature.type.ts.

Why?
Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across a company provides tremendous efficiency.

Why?
The naming conventions should help find desired code faster and make it easier to understand.

Why?
Names of folders and files should clearly convey their intent. For example, app/heroes/hero-list.component.ts may contain a component that manages a list of heroes.

**[⬆ back to top](#table-of-contents)**

## Separate file names with dots and dashes

Style 02-02
Do use dashes to separate words in the descriptive name.

Do use dots to separate the descriptive name from the type.

Do use consistent type names for all components following a pattern that describes the component's feature then its type. A recommended pattern is feature.type.ts.

Do use conventional type names including .service, .component, .pipe, .module, and .directive. Invent additional type names if you must but take care not to create too many.

Why?
Type names provide a consistent way to quickly identify what is in the file.

Why?
Type names make it easy to find a specific file type using an editor or IDE's fuzzy search techniques.

Why?
Unabbreviated type names such as .service are descriptive and unambiguous. Abbreviations such as .srv, .svc, and .serv can be confusing.

Why?
Type names provide pattern matching for any automated tasks.

**[⬆ back to top](#table-of-contents)**

## Symbols and file names

Do use consistent names for all assets named after what they represent.

Do use upper camel case for class names.

Do match the name of the symbol to the name of the file.

Do append the symbol name with the conventional suffix (such as Component, Directive, Module, Pipe, or Service) for a thing of that type.

Do give the filename the conventional suffix (such as .component.ts, .directive.ts, .module.ts, .pipe.ts, or .service.ts) for a file of that type.

Why?
Consistent conventions make it easy to quickly identify and reference assets of different types.

<table>
  <thead>
    <tr>
      <th>Symbol Name</th>
      <th>File Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>@Component({ … })<br>export class AppComponent { }</td>
      <td>app.component.ts</td>
    </tr>
    <tr>
      <td>@Component({ … })<br>export class HeroesComponent { }</td>
      <td>heroes.component.ts</td>
    </tr>
    <tr>
      <td>@Component({ … })<br>export class HeroListComponent { }</td>
      <td>hero-list.component.ts</td>
    </tr>
    <tr>
      <td>@Component({ … })<br>export class HeroDetailComponent { }</td>
      <td>hero-detail.component.ts</td>
    </tr>
    <tr>
      <td>@Directive({ … })<br>export class ValidationDirective { }</td>
      <td>validation.directive.ts</td>
    </tr>
    <tr>
      <td>@NgModule({ … })<br>export class AppModule</td>
      <td>app.module.ts</td>
    </tr>
    <tr>
      <td>@Pipe({ name: 'initCaps' })<br>export class InitCapsPipe implements PipeTransform { }</td>
      <td>init-caps.pipe.ts</td>
    </tr>
    <tr>
      <td>@Injectable()<br>export class UserProfileService { }</td>
      <td>user-profile.service.ts</td>
    </tr>
  </tbody>
</table>

**[⬆ back to top](#table-of-contents)**

## Service names

Style 02-04
Do use consistent names for all services named after their feature.

Do suffix a service class name with Service. For example, something that gets data or heroes should be called a DataService or a HeroService.

A few terms are unambiguously services. They typically indicate agency by ending in "-er". You may prefer to name a service that logs messages Logger rather than LoggerService. Decide if this exception is agreeable in your project. As always, strive for consistency.

Why?
Provides a consistent way to quickly identify and reference services.

Why?
Clear service names such as Logger do not require a suffix.

Why?
Service names such as Credit are nouns and require a suffix and should be named with a suffix when it is not obvious if it is a service or something else.

<table>
  <thead>
    <tr>
      <th>Symbol Name</th>
      <th>File Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>@Injectable()<br>export class HeroDataService { }</td>
      <td>hero-data.service.ts</td>
    </tr>
    <tr>
      <td>@Injectable()<br>export class CreditService { }</td>
      <td>credit.service.ts</td>
    </tr>
    <tr>
      <td>@Injectable()<br>export class Logger { }</td>
      <td>logger.service.ts</td>
    </tr>
  </tbody>
</table>

**[⬆ back to top](#table-of-contents)**

## Bootstrapping

Style 02-05
Do put bootstrapping and platform logic for the application in a file named main.ts.

Do include error handling in the bootstrapping logic.

Avoid putting application logic in main.ts. Instead, consider placing it in a component or service.

Why?
Follows a consistent convention for the startup logic of an app.

Why?
Follows a familiar convention from other technology platforms.

2. main.ts:

   ```
   import {AppComponent} from './app/app.component';
   import {bootstrapApplication} from '@angular/platform-browser';
   bootstrapApplication(AppComponent);
   ```

**[⬆ back to top](#table-of-contents)**

## Component selectors

Do use dashed-case or kebab-case for naming the element selectors of components.

Why?
Keeps the element names consistent with the specification for Custom Elements.

1. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'tohHeroButton',
   templateUrl: './hero-button.component.html',
   })
   export class HeroButtonComponent {}
   ```

2. TS:

   ```
   @Component({
   selector: 'toh-hero-button',
   templateUrl: './hero-button.component.html',
   })
   export class HeroButtonComponent {}
   ```

**[⬆ back to top](#table-of-contents)**

## Component custom prefix

Style 02-07
Do use a hyphenated, lowercase element selector value; for example, admin-users.

Do use a prefix that identifies the feature area or the application itself.

Why?
Prevents element name collisions with components in other applications and with native HTML elements.

Why?
Makes it easier to promote and share the component in other applications.

Why?
Components are easy to identify in the DOM.

1. app/heroes/hero.component.ts:

   ```
   /* avoid */
   // HeroComponent is in the Tour of Heroes feature
   @Component({
   selector: 'hero',
   template: '',
   })
   export class HeroComponent {}
   ```

2. app/users/users.component.ts:

   ```
   /* avoid */
   // UsersComponent is in an Admin feature
   @Component({
   selector: 'users',
   template: '',
   })
   export class UsersComponent {}
   ```

3. app/heroes/hero.component.ts:

   ```
   @Component({
   ...
   selector: 'toh-hero',
   })
   export class HeroComponent {}
   ```

4. app/users/users.component.ts:

   ```
   @Component({
   ...
   selector: 'admin-users',
   })
   export class UsersComponent {}
   ```

**[⬆ back to top](#table-of-contents)**

## Directive selectors

Style 02-06
Do Use lower camel case for naming the selectors of directives.

Why?
Keeps the names of the properties defined in the directives that are bound to the view consistent with the attribute names.

Why?
The Angular HTML parser is case-sensitive and recognizes lower camel case.

**[⬆ back to top](#table-of-contents)**

## Directive custom prefix

Style 02-08
Do spell non-element selectors in lower camel case unless the selector is meant to match a native HTML attribute.

Don't prefix a directive name with ng because that prefix is reserved for Angular and using it could cause bugs that are difficult to diagnose.

Why?
Prevents name collisions.

Why?
Directives are easily identified.

1. app/shared/validate.directive.ts:

   ```
   /* avoid */
   @Directive({
   selector: '[validate]',
   })
   export class ValidateDirective {}
   ```

2. app/shared/validate.directive.ts:

   ```
   @Directive({
   selector: '[tohValidate]',
   })
   export class ValidateDirective {}
   ```

**[⬆ back to top](#table-of-contents)**

## Pipe names

Style 02-09
Do use consistent names for all pipes, named after their feature. The pipe class name should use UpperCamelCase (the general convention for class names), and the corresponding name string should use lowerCamelCase. The name string cannot use hyphens ("dash-case" or "kebab-case").

Why?
Provides a consistent way to quickly identify and reference pipes.

<table>
  <thead>
    <tr>
      <th>Symbol Name</th>
      <th>File Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>@Pipe({ name: 'ellipsis' })<br>export class EllipsisPipe implements PipeTransform { }</td>
      <td>ellipsis.pipe.ts</td>
    </tr>
    <tr>
      <td>@Pipe({ name: 'initCaps' })<br>export class InitCapsPipe implements PipeTransform { }</td>
      <td>init-caps.pipe.ts</td>
    </tr>
  </tbody>
</table>

**[⬆ back to top](#table-of-contents)**

## Unit test file names

Style 02-10
Do name test specification files the same as the component they test.

Do name test specification files with a suffix of .spec.

Why?
Provides a consistent way to quickly identify tests.

Why?
Provides pattern matching for karma or other test runners.

<table>
  <thead>
    <tr>
      <th>Test Type</th>
      <th>File Names</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Components</td>
      <td>heroes.component.spec.ts<br>hero-list.component.spec.ts<br>hero-detail.component.spec.ts</td>
    </tr>
    <tr>
      <td>Services</td>
      <td>logger.service.spec.ts<br>hero.service.spec.ts<br>filter-text.service.spec.ts</td>
    </tr>
    <tr>
      <td>Pipes</td>
      <td>ellipsis.pipe.spec.ts<br>init-caps.pipe.spec.ts</td>
    </tr>
  </tbody>
</table>

**[⬆ back to top](#table-of-contents)**

## Client Component Placement Strategy in Next.js

To ensure optimal performance and efficient rendering in a Next.js application, it's crucial to structure your components in a way that maximizes the benefits of server-side rendering and client-side interactivity. One effective approach is to place client components as lower leaves in the component tree hierarchy. This strategy leverages Next.js's ability to pre-render pages at build time or request time while allowing dynamic components to handle interactive features on the client side.

### Why Place Client Components as Lower Leaves

Placing client components, such as those utilizing dynamic data fetching or state management, as lower leaves in the component tree offers several advantages:

- **Enhanced Initial Page Load**: Components rendered on the server provide faster initial page load times as they are pre-rendered and delivered to the client.
- **Improved SEO and Accessibility**: Server-rendered content is accessible to search engines and users with JavaScript disabled, ensuring better SEO performance and accessibility compliance.

- **Progressive Enhancement**: By loading basic content server-side and enhancing it with client-side interactivity, you provide a smooth user experience that gracefully degrades for users with slower connections or less capable devices.

### Example Implementation

```jsx
import React from "react";
import { useEffect, useState } from "react";

const DynamicContent = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    // Example: Fetch data dynamically on the client side
    const response = await fetch("https://api.example.com/data");
    const newData = await response.json();
    setData(newData);
  };

  return (
    <div>
      {data.map((item) => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
};

export default DynamicContent;
```

In this example, `DynamicContent` fetches data dynamically on the client side, making it suitable for placement as a lower leaf component in a Next.js application. This ensures that server-side rendering optimizes initial page load performance while maintaining flexibility for client-side interactions.
