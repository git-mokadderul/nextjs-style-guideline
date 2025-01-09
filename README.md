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

### **Application structure and NgModules**

12. [Overall structural guidelines](#Overall-structural-guidelines)
13. [Folders-by-feature structure](#Folders-by-feature-structure)
14. [App root module](#App-root-module)
15. [Feature modules](#Feature-modules)
16. [Shared feature module](#Shared-feature-module)
17. [Lazy Loaded folders](#Lazy-Loaded-folders)

### **Components**

18. [Components as elements](#Components-as-elements)
19. [Extract templates and styles to their own files](#Extract-templates-and-styles-to-their-own-files)
20. [Decorate input and output properties](#Decorate-input-and-output-properties)
21. [Avoid aliasing inputs and outputs](#Avoid-aliasing-inputs-and-outputs)
22. [Delegate complex component logic to services](#Delegate-complex-component-logic-to-services)
23. [Don't prefix output properties](#Don't-prefix-output-properties)
24. [Put presentation logic in the component class](#Put-presentation-logic-in-the-component-class)
25. [Initialize inputs](#Initialize-inputs)

### **Directives**

26. [Use directives to enhance an element](#Use-directives-to-enhance-an-element)
27. [HostListener/HostBinding decorators versus host metadata](#HostListener/HostBinding-decorators-versus-host-metadata)

### **Services**

28. [Services are singletons](#Services-are-singletons)
29. [Providing a service](#Providing-a-service)
30. [Use the @Injectable() class decorator](<#Use-the-@Injectable()-class-decorator>)

### **Data Services**

28. [Talk to the server through a service](#Talk-to-the-server-through-a-service)

### **Lifecycle hooks**

28. [Implement lifecycle hook interfaces](#Implement-lifecycle-hook-interfaces)

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

1. main.ts:

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

# Application structure and NgModules

Have a near-term view of implementation and a long-term vision. Start small but keep in mind where the application is heading.

All of the application's code goes in a folder named src. All feature areas are in their own folder.

All content is one asset per file. Each component, service, and pipe is in its own file. All third party vendor scripts are stored in another folder and not in the src folder. Use the naming conventions for files in this guide.

## Overall structural guidelines

Style 04-06
Do start small but keep in mind where the application is heading down the road.

Do have a near term view of implementation and a long term vision.

Do put all of the application's code in a folder named src.

Consider creating a folder for a component when it has multiple accompanying files (.ts, .html, .css, and .spec).

Why?
Helps keep the application structure small and easy to maintain in the early stages, while being easy to evolve as the application grows.

Why?
Components often have four files (for example, _.html, _.css, _.ts, and _.spec.ts) and can clutter a folder quickly.

Here is a compliant folder and file structure:

```
project root
├── src
│ ├── app
│ │ ├── core
│ │ │ └── exception.service.ts|spec.ts
│ │ │ └── user-profile.service.ts|spec.ts
│ │ ├── heroes
│ │ │ ├── hero
│ │ │ │ └── hero.component.ts|html|css|spec.ts
│ │ │ ├── hero-list
│ │ │ │ └── hero-list.component.ts|html|css|spec.ts
│ │ │ ├── shared
│ │ │ │ └── hero-button.component.ts|html|css|spec.ts
│ │ │ │ └── hero.model.ts
│ │ │ │ └── hero.service.ts|spec.ts
│ │ │ └── heroes.component.ts|html|css|spec.ts
│ │ │ └── heroes.routes.ts
│ │ ├── shared
│ │ │ └── init-caps.pipe.ts|spec.ts
│ │ │ └── filter-text.component.ts|spec.ts
│ │ │ └── filter-text.service.ts|spec.ts
│ │ ├── villains
│ │ │ ├── villain
│ │ │ │ └── …
│ │ │ ├── villain-list
│ │ │ │ └── …
│ │ │ ├── shared
│ │ │ │ └── …
│ │ │ └── villains.component.ts|html|css|spec.ts
│ │ │ └── villains.module.ts
│ │ │ └── villains-routing.module.ts
│ │ └── app.component.ts|html|css|spec.ts
│ │ └── app.routes.ts
│ └── main.ts
│ └── index.html
│ └── …
└── node_modules/…
└── …
```

**[⬆ back to top](#table-of-contents)**

## Folders-by-feature structure

Style 04-07
Do create folders named for the feature area they represent.

Why?
A developer can locate the code and identify what each file represents at a glance. The structure is as flat as it can be and there are no repetitive or redundant names.

Why?
Helps reduce the application from becoming cluttered through organizing the content.

Why?
When there are a lot of files, for example 10+, locating them is easier with a consistent folder structure and more difficult in a flat structure.

**[⬆ back to top](#table-of-contents)**

## App root module

Style 04-08
Do create an NgModule in the application's root folder, for example, in /src/app if creating a NgModule based app.

Why?
Every NgModule based application requires at least one root NgModule.

Consider naming the root module app.module.ts.

Why?
Makes it easier to locate and identify the root module.

1. app/app.module.ts:

   ```
   import {NgModule} from '@angular/core';
   import {BrowserModule} from '@angular/platform-browser';
   ...
   import {AppComponent} from './app.component';
   import {HeroesComponent} from './heroes/heroes.component';
   @NgModule({
   imports: [
    BrowserModule,
   ...
   ],
   declarations: [AppComponent, HeroesComponent],
   exports: [AppComponent],
   })
   export class AppModule {}
   ```

**[⬆ back to top](#table-of-contents)**

## Feature modules

Style 04-09
Do create an NgModule for all distinct features in an application; for example, a Heroes feature.

Do place the feature module in the same named folder as the feature area; for example, in app/heroes.

Do name the feature module file reflecting the name of the feature area and folder; for example, app/heroes/heroes.module.ts.

Do name the feature module symbol reflecting the name of the feature area, folder, and file; for example, app/heroes/heroes.module.ts defines HeroesModule.

Why?
A feature module can expose or hide its implementation from other modules.

Why?
A feature module identifies distinct sets of related components that comprise the feature area.

Why?
A feature module can easily be routed to both eagerly and lazily.

Why?
A feature module defines clear boundaries between specific functionality and other application features.

Why?
A feature module helps clarify and make it easier to assign development responsibilities to different teams.

Why?
A feature module can easily be isolated for testing.

**[⬆ back to top](#table-of-contents)**

## Shared feature module

Style 04-10
Do create a feature module named SharedModule in a shared folder; for example, app/shared/shared.module.ts defines SharedModule.

Do declare components, directives, and pipes in a shared module when those items will be re-used and referenced by the components declared in other feature modules.

Consider using the name SharedModule when the contents of a shared module are referenced across the entire application.

Consider not providing services in shared modules. Services are usually singletons that are provided once for the entire application or in a particular feature module. There are exceptions, however. For example, in the sample code that follows, notice that the SharedModule provides FilterTextService. This is acceptable here because the service is stateless;that is, the consumers of the service aren't impacted by new instances.

Do import all modules required by the assets in the SharedModule; for example, CommonModule and FormsModule.

Why?
SharedModule will contain components, directives, and pipes that may need features from another common module; for example, ngFor in CommonModule.

Do declare all components, directives, and pipes in the SharedModule.

Do export all symbols from the SharedModule that other feature modules need to use.

Why?
SharedModule exists to make commonly used components, directives, and pipes available for use in the templates of components in many other modules.

Avoid specifying app-wide singleton providers in a SharedModule. Intentional singletons are OK. Take care.

Why?
A lazy loaded feature module that imports that shared module will make its own copy of the service and likely have undesirable results.

Why?
You don't want each module to have its own separate instance of singleton services. Yet there is a real danger of that happening if the SharedModule provides a service.

```
project root
├──src
├──├──app
├──├──├── shared
├──├──├──└── shared.module.ts
├──├──├──└── init-caps.pipe.ts|spec.ts
├──├──├──└── filter-text.component.ts|spec.ts
├──├──├──└── filter-text.service.ts|spec.ts
├──├──└── app.component.ts|html|css|spec.ts
├──├──└── app.module.ts
├──├──└── app-routing.module.ts
├──└── main.ts
├──└── index.html
└── …
```

1. Shared.module.ts:

   ```
   import {Injectable} from '@angular/core';
   @Injectable()
   export class FilterTextService {
   constructor() {
    console.log('Created an instance of FilterTextService');
   }
   filter(data: string, props: Array<string>, originalList: Array<any>) {
    let filteredList: any[];
    if (data && props && originalList) {
      data = data.toLowerCase();
      const filtered = originalList.filter((item) => {
        let match = false;
        for (const prop of props) {
          if (item[prop].toString().toLowerCase().indexOf(data) > -1) {
            match = true;
            break;
          }
        }
        return match;
      });
      filteredList = filtered;
    } else {
      filteredList = originalList;
    }
    return filteredList;
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Lazy Loaded folders

A distinct application feature or workflow may be lazy loaded or loaded on demand rather than when the application starts.

Do put the contents of lazy loaded features in a lazy loaded folder. A typical lazy loaded folder contains a routing component, its child components, and their related assets.

Why?
The folder makes it easy to identify and isolate the feature content.

**[⬆ back to top](#table-of-contents)**

# Components

## Components as elements

Style 05-03
Consider giving components an element selector, as opposed to attribute or class selectors.

Why?
Components have templates containing HTML and optional Angular template syntax. They display content. Developers place components on the page as they would native HTML elements and web components.

Why?
It is easier to recognize that a symbol is a component by looking at the template's html.

1. app/heroes/hero-button/hero-button.component.ts:

   ```
   /* avoid */
   @Component({
   selector: '[tohHeroButton]',
   templateUrl: './hero-button.component.html',
   })
   export class HeroButtonComponent {}
   ```

2. app/app.component.html:

   ```
   <!-- avoid -->
   <div tohHeroButton></div>
   ```

3. TS:

   ```
   @Component({
   selector: 'toh-hero-button',
   templateUrl: './hero-button.component.html',
   })
   export class HeroButtonComponent {}
   ```

**[⬆ back to top](#table-of-contents)**

## Extract templates and styles to their own files

Do extract templates and styles into a separate file, when more than 3 lines.

Do name the template file [component-name].component.html, where [component-name] is the component name.

Do name the style file [component-name].component.css, where [component-name] is the component name.

Do specify component-relative URLs, prefixed with ./.

Why?
Large, inline templates and styles obscure the component's purpose and implementation, reducing readability and maintainability.

Why?
In most editors, syntax hints and code snippets aren't available when developing inline templates and styles. The Angular TypeScript Language Service (forthcoming) promises to overcome this deficiency for HTML templates in those editors that support it; it won't help with CSS styles.

Why?
A component relative URL requires no change when you move the component files, as long as the files stay together.

Why?
The ./ prefix is standard syntax for relative URLs; don't depend on Angular's current ability to do without that prefix.

1. app/heroes/heroes.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'toh-heroes',
   template: `
    <div>
      <h2>My Heroes</h2>
      <ul class="heroes">
        @for (hero of heroes | async; track hero) {
          <li (click)="selectedHero=hero">
            <span class="badge">{{hero.id}}</span> {{hero.name}}
          </li>
        }
      </ul>
      @if (selectedHero) {
        <div>
          <h2>{{selectedHero.name | uppercase}} is my hero</h2>
        </div>
      }
    </div>
   `,
   styles: [
    `
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .heroes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .heroes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color: #607D8B;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
   `,
   ],
   imports: [NgFor, NgIf, AsyncPipe, UpperCasePipe],
   })
   export class HeroesComponent {
   heroes: Observable<Hero[]>;
   selectedHero!: Hero;
   constructor(private heroService: HeroService) {
    this.heroes = this.heroService.getHeroes();
   }
   }
   ```

2. TS:

   ```
   @Component({
   selector: 'toh-heroes',
   templateUrl: './heroes.component.html',
   styleUrls: ['./heroes.component.css'],
   imports: [NgFor, NgIf, AsyncPipe, UpperCasePipe],
   })
   export class HeroesComponent {
   heroes: Observable<Hero[]>;
   selectedHero!: Hero;
   constructor(private heroService: HeroService) {
    this.heroes = this.heroService.getHeroes();
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Decorate input and output properties

Style 05-12
Do use the @Input() and @Output() class decorators instead of the inputs and outputs properties of the @Directive and @Component metadata:

Consider placing @Input() or @Output() on the same line as the property it decorates.

Why?
It is easier and more readable to identify which properties in a class are inputs or outputs.

Why?
If you ever need to rename the property or event name associated with @Input() or @Output(), you can modify it in a single place.

Why?
The metadata declaration attached to the directive is shorter and thus more readable.

Why?
Placing the decorator on the same line usually makes for shorter code and still easily identifies the property as an input or output. Put it on the line above when doing so is clearly more readable.

1. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button"></button>`,
   inputs: ['label'],
   outputs: ['heroChange'],
   })
   export class HeroButtonComponent {
   heroChange = new EventEmitter<any>();
   label: string = '';
   }
   ```

2. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button">{{label}}</button>`,
   })
   export class HeroButtonComponent {
   @Output() heroChange = new EventEmitter<any>();
   @Input() label = '';
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Avoid aliasing inputs and outputs

Avoid input and output aliases except when it serves an important purpose.

Why?
Two names for the same property (one private, one public) is inherently confusing.

Why?
You should use an alias when the directive name is also an input property, and the directive name doesn't describe the property.

1. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   /* avoid pointless aliasing */
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button">{{label}}</button>`,
   })
   export class HeroButtonComponent {
   // Pointless aliases
   @Output('heroChangeEvent') heroChange = new EventEmitter<any>();
   @Input('labelAttribute') label!: string;
   }
   ```

2. app/app.component.html:

   ```
   <!-- avoid -->
   <toh-hero-button labelAttribute="OK" (changeEvent)="doSomething()">
   </toh-hero-button>
   ```

3. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button" >{{label}}</button>`,
   })
   export class HeroButtonComponent {
   // No aliases
   @Output() heroChange = new EventEmitter<any>();
   @Input() label = '';
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Delegate complex component logic to services

Style 05-15
Do limit logic in a component to only that required for the view. All other logic should be delegated to services.

Do move reusable logic to services and keep components simple and focused on their intended purpose.

Why?
Logic may be reused by multiple components when placed within a service and exposed as a function.

Why?
Logic in a service can more easily be isolated in a unit test, while the calling logic in the component can be easily mocked.

Why?
Removes dependencies and hides implementation details from the component.

Why?
Keeps the component slim, trim, and focused.

1. app/heroes/hero-list/hero-list.component.ts:

   ```
   /* avoid */
   import {Component, OnInit} from '@angular/core';
   import {HttpClient} from '@angular/common/http';
   import {Observable} from 'rxjs';
   import {catchError, finalize} from 'rxjs/operators';
   import {Hero} from '../shared/hero.model';
   const heroesUrl = 'http://angular.io';
   @Component({
   selector: 'toh-hero-list',
   template: `...`,
   })
   export class HeroListComponent implements OnInit {
   heroes: Hero[];
   constructor(private http: HttpClient) {
    this.heroes = [];
   }
   getHeroes() {
    this.heroes = [];
    this.http
      .get(heroesUrl)
      .pipe(
        catchError(this.catchBadResponse),
        finalize(() => this.hideSpinner()),
      )
      .subscribe((heroes: Hero[]) => (this.heroes = heroes));
   }
   ngOnInit() {
    this.getHeroes();
   }
   private catchBadResponse(err: any, source: Observable<any>) {
    // log and handle the exception
    return new Observable();
   }
   private hideSpinner() {
    // hide the spinner
   }
   }
   ```

2. app/heroes/hero-list/hero-list.component.ts:

   ```
   import {Component, OnInit} from '@angular/core';
   import {Hero, HeroService} from '../shared';
   @Component({
   selector: 'toh-hero-list',
   template: `...`,
   })
   export class HeroListComponent implements OnInit {
   heroes: Hero[] = [];
   constructor(private heroService: HeroService) {}
   getHeroes() {
    this.heroes = [];
    this.heroService.getHeroes().subscribe((heroes) => (this.heroes = heroes));
   }
   ngOnInit() {
    this.getHeroes();
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Don't prefix output properties

Style 05-16
Do name events without the prefix on.

Do name event handler methods with the prefix on followed by the event name.

Why?
This is consistent with built-in events such as button clicks.

Why?
Angular allows for an alternative syntax on-\*. If the event itself was prefixed with on this would result in an on-onEvent binding expression.

1. app/heroes/hero.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'toh-hero',
   template: `...`,
   })
   export class HeroComponent {
   @Output() onSavedTheDay = new EventEmitter<boolean>();
   }
   ```

2. app/app.component.html:

   ```
   <!-- avoid -->
   <toh-hero (onSavedTheDay)="onSavedTheDay($event)"></toh-hero>
   ```

3. TS:

   ```
   export class HeroComponent {
   @Output() savedTheDay = new EventEmitter<boolean>();
   } @Component({
   selector: 'toh-hero-button',
   template: `<button type="button" >{{label}}</button>`,
   })
   export class HeroButtonComponent {
   // No aliases
   @Output() heroChange = new EventEmitter<any>();
   @Input() label = '';
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Put presentation logic in the component class

Style 05-17
Do put presentation logic in the component class, and not in the template.

Why?
Logic will be contained in one place (the component class) instead of being spread in two places.

Why?
Keeping the component's presentation logic in the class instead of the template improves testability, maintainability, and reusability.

1. app/heroes/hero-list/hero-list.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'toh-hero-list',
   template: `
    <section>
      Our list of heroes:
      @for (hero of heroes; track hero) {
        <toh-hero [hero]="hero"></toh-hero>
      }
      Total powers: {{totalPowers}}<br>
      Average power: {{totalPowers / heroes.length}}
    </section>
   `,
   imports: [NgFor, HeroComponent],
   })
   export class HeroListComponent {
   heroes: Hero[] = [];
   totalPowers: number = 0;
   }
   ```

2. app/heroes/hero-list/hero-list.component.ts:

   ```
   @Component({
   selector: 'toh-hero-list',
   template: `
    <section>
      Our list of heroes:
      @for (hero of heroes; track hero) {
        <toh-hero [hero]="hero"></toh-hero>
      }
      Total powers: {{totalPowers}}<br>
      Average power: {{avgPower}}
    </section>
   `,
   imports: [NgFor, HeroComponent],
   })
   export class HeroListComponent {
   heroes: Hero[];
   totalPowers = 0;
   ...
   get avgPower() {
    return this.totalPowers / this.heroes.length;
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Initialize inputs

Style 05-18
TypeScript's --strictPropertyInitialization compiler option ensures that a class initializes its properties during construction. When enabled, this option causes the TypeScript compiler to report an error if the class does not set a value to any property that is not explicitly marked as optional.

By design, Angular treats all @Input properties as optional. When possible, you should satisfy --strictPropertyInitialization by providing a default value.

1. app/heroes/hero/hero.component.ts:

   ```
   @Component({
   selector: 'toh-hero',
   template: `...`,
   })
   export class HeroComponent {
   @Input() id = 'default_id';
   }
   ```

If the property is hard to construct a default value for, use ? to explicitly mark the property as optional.

2. app/heroes/hero/hero.component.ts:

   ```
   @Component({
   selector: 'toh-hero',
   template: `...`,
   })
   export class HeroComponent {
   @Input() id?: string;
   process() {
    if (this.id) {
      // ...
    }
   }
   }
   ```

You may want to have a required @Input field, meaning all your component users are required to pass that attribute. In such cases, use a default value. Just suppressing the TypeScript error with ! is insufficient and should be avoided because it will prevent the type checker from ensuring the input value is provided.

3. app/heroes/hero/hero.component.ts:

```
@Component({
  selector: 'toh-hero',
  template: `...`,
})
export class HeroComponent {
  // The exclamation mark suppresses errors that a property is
  // not initialized.
  // Ignoring this enforcement can prevent the type checker
  // from finding potential issues.
  @Input() id!: string;
}
```

**[⬆ back to top](#table-of-contents)**

# Directives

## Use directives to enhance an element

Style 06-01
Do use attribute directives when you have presentation logic without a template.

Why?
Attribute directives don't have an associated template.

Why?
An element may have more than one attribute directive applied.

1. app/shared/highlight.directive.ts:

   ```
   @Directive({
   selector: '[tohHighlight]',
   })
   export class HighlightDirective {
   @HostListener('mouseover') onMouseEnter() {
    // do highlight work
   }
   }
   ```

2. app/app.component.html:

   ```
   <div tohHighlight>Bombasta</div>
   ```

**[⬆ back to top](#table-of-contents)**

## HostListener/HostBinding decorators versus host metadata

Style 06-03
Consider preferring the @HostListener and @HostBinding to the host property of the @Directive and @Component decorators.

Do be consistent in your choice.

Why?
The property associated with @HostBinding or the method associated with @HostListener can be modified only in a single place —in the directive's class. If you use the host metadata property, you must modify both the property/method declaration in the directive's class and the metadata in the decorator associated with the directive.

1. app/shared/validator.directive.ts:

   ```
   import {Directive, HostBinding, HostListener} from '@angular/core';
   @Directive({
   selector: '[tohValidator]',
   })
   export class ValidatorDirective {
   @HostBinding('attr.role') role = 'button';
   @HostListener('mouseenter') onMouseEnter() {
    // do work
   }
   }
   ```

Compare with the less preferred host metadata alternative.

Why?
The host metadata is only one term to remember and doesn't require extra ES imports.

2. app/shared/validator2.directive.ts:

   ```
   import {Directive} from '@angular/core';
   @Directive({
   selector: '[tohValidator2]',
   host: {
    '[attr.role]': 'role',
    '(mouseenter)': 'onMouseEnter()',
   },
   })
   export class Validator2Directive {
   role = 'button';
   onMouseEnter() {
    // do work
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

# Services

## Services are singletons

Style 07-01
Do use services as singletons within the same injector. Use them for sharing data and functionality.

Why?
Services are ideal for sharing methods across a feature area or an app.

Why?
Services are ideal for sharing stateful in-memory data.

1. app/heroes/shared/hero.service.ts:

   ```
   export class HeroService {
   constructor(private http: HttpClient) {}
   getHeroes() {
    return this.http.get<Hero[]>('api/heroes');
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**

## Providing a service

Do provide a service with the application root injector in the @Injectable decorator of the service.

Why?
The Angular injector is hierarchical.

Why?
When you provide the service to a root injector, that instance of the service is shared and available in every class that needs the service. This is ideal when a service is sharing methods or state.

Why?
When you register a service in the @Injectable decorator of the service, optimization tools such as those used by the Angular CLI's production builds can perform tree shaking and remove services that aren't used by your app.

Why?
This is not ideal when two different components need different instances of a service. In this scenario it would be better to provide the service at the component level that needs the new and separate instance.

1. src/app/treeshaking/service.ts:

   ```
   import {Injectable} from '@angular/core';
   @Injectable({
   providedIn: 'root',
   })
   export class Service {}
   ```

**[⬆ back to top](#table-of-contents)**

## Use the @Injectable() class decorator

Style 07-04
Do use the @Injectable() class decorator instead of the @Inject parameter decorator when using types as tokens for the dependencies of a service.

Why?
The Angular Dependency Injection (DI) mechanism resolves a service's own dependencies based on the declared types of that service's constructor parameters.

Why?
When a service accepts only dependencies associated with type tokens, the @Injectable() syntax is much less verbose compared to using @Inject() on each individual constructor parameter.

1. app/heroes/shared/hero-arena.service.ts:

   ```
   /* avoid */
   export class HeroArena {
   constructor(
    @Inject(HeroService) private heroService: HeroService,
    @Inject(HttpClient) private http: HttpClient,
   ) {}
   }
   ```

2. app/heroes/shared/hero-arena.service.ts:

   ```
   @Injectable()
   export class HeroArena {
   constructor(
    private heroService: HeroService,
    private http: HttpClient,
   ) {}
   ...
   }
   ```

**[⬆ back to top](#table-of-contents)**

# Data Services

## Talk to the server through a service

Style 08-01
Do refactor logic for making data operations and interacting with data to a service.

Do make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

Why?
The component's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the component be simpler and more focused on the view.

Why?
This makes it easier to test (mock or real) the data calls when testing a component that uses a data service.

Why?
The details of data management, such as headers, HTTP methods, caching, error handling, and retry logic, are irrelevant to components and other data consumers.

A data service encapsulates these details. It's easier to evolve these details inside the service without affecting its consumers. And it's easier to test the consumers with mock service implementations.

**[⬆ back to top](#table-of-contents)**

# Lifecycle hooks

Use Lifecycle hooks to tap into important events exposed by Angular.

## Implement lifecycle hook interfaces

Style 09-01
Do implement the lifecycle hook interfaces.

Why?
Lifecycle interfaces prescribe typed method signatures. Use those signatures to flag spelling and syntax mistakes.

1. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   /* avoid */
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button">OK</button>`,
   })
   export class HeroButtonComponent {
   onInit() {
    // misspelled
    console.log('The component is initialized');
   }
   }
   ```

2. app/heroes/shared/hero-button/hero-button.component.ts:

   ```
   @Component({
   selector: 'toh-hero-button',
   template: `<button type="button">OK</button>`,
   })
   export class HeroButtonComponent implements OnInit {
   ngOnInit() {
    console.log('The component is initialized');
   }
   }
   ```

**[⬆ back to top](#table-of-contents)**
