# Chapter 2: The Root Component

## 1 Project Files

The Angular CLI generates several folders and a number of files including source code, enviornment settings, testing and etc. The [Angular Getting Started Document](https://angular.io/guide/quickstart) describes each file in detail.

The built-in web server, started by the `ng serve` command, watches project changes and will rebuild the project when there is a change.

## 2 The Root Component

In Angular, a web page are built from one or more components. A component is similar to a native HTML element that has a view and some interactive behaviors. Similar to a typical HTML page, a component usualy consists of three files: an HTML template file, a CSS style file, and a TypeScript code file. A website has a home page and an Angular application has a root component.

Use your IDE to open the `angular-tour-of-heroes` folder. The application source code is in the `src/` folder. The `src/app` folder has the three files for the root component.

- `app.component.ts`: the TS code file that defines the component class.
- `app.component.html`: the HTML template file.
- `app.component.css`: the CSS style file.

The component class file `app.component.ts` defines the data and interaction logic used by the HTML template file.

The `app.component.html` is a standard HTML file that defines the HTML elements and presents data from the component class.

The `app.component.css` is used to define local styles that only affect the view of this component. To define application wide style, use the `src/styles.css` file.

## 3 Changing the Root Component

Just change the title line while keep other lines untouched as the following in `app.component.ts` file.

```ts
//... other lines stay the same
export class AppComponent {
  title = 'Tour of Heroes'
}
```

Replace the content of the `app.component.html` to have the following content:

```html
<h1>{{title}}</h1>
```

The double curly braces in `{{title}}` means that the `title` is a TS expression, evaluate it and display its output in its place. It turns out that `title` is a property of the component class and its value `'Tour of Heroes'` is used as the content of the HTML `h1` tag.

Edit the `src/styles.css` file to have the following styles.

```css
/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2,
h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body,
input[text],
button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

If you keep your browser open, all changes are reflected in your browser lively. You should have the following view:

![Tour of Heroes Page](./ch02-1.png)
