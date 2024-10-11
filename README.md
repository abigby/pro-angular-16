# Manning Source Code

This repository accompanies *Pro Angular 16* by Adam Freeman.

Download the files as a zip using the green button or clone the repository to your machine using Git.

## Releases

This repository corresponds to the examples in the published book, without corrections or updates. See [this](errata.md) file for serious issues that are likely to prevent the examples from working as described in the book. See [this](typos.md) file for small mistakes that I intend to correct in the next edition.

## Contributions

If you find an error, please contact the author using the email address published in the book. Please do not create pull requests because they will not be accepted. Please do not create issues because the author will not respond to them.



Here are sample answers a senior Angular developer might give to the questions:

What architectural patterns do you recommend when building large-scale Angular applications, and how do you ensure maintainability over time?

I follow a modular architecture, breaking down the app into core, shared, and feature modules. I ensure each module is self-contained and only exposes what's necessary. I use the Service-Oriented Architecture (SOA) model to isolate business logic into services and keep components lean. To maintain scalability and ease of refactoring, I rely on a layered architecture (data, service, presentation layers) and clearly define interfaces between components.
How do you handle state management in Angular applications? Do you prefer using NgRx, Akita, or a custom solution? Why?

I prefer NgRx for large, complex applications because it follows the Redux pattern, making state predictable, testable, and time-travel debugging is a big win. For smaller apps, though, I may go with a simpler service-based state management approach using BehaviorSubjects or RxJS, as it’s lighter. Akita is another good option if you need flexibility and less boilerplate than NgRx.
Can you describe a complex performance issue you've encountered in an Angular app and how you resolved it?

Once, I faced a performance issue with an app that had too many watchers due to a large number of components. It was causing slow rendering. I identified that unnecessary change detection was happening using Angular’s ChangeDetectionStrategy.OnPush, which reduced the frequency of checks. Additionally, I optimized how large datasets were rendered by using virtual scrolling in the UI.
What strategies do you use to optimize an Angular application's initial load time and overall performance?

I implement lazy loading for feature modules to reduce the initial bundle size. I also use route preloading to balance performance. For assets, I rely on techniques like tree-shaking and AOT (Ahead of Time) compilation. Minifying and compressing JavaScript, CSS, and images is another optimization technique. I use tools like webpack-bundle-analyzer to analyze bundle sizes and identify unnecessary dependencies.
How do you approach testing in Angular, especially with complex components or services? What tools do you prefer (e.g., Jest, Cypress, Karma, Protractor)?

For unit testing, I use Jest because it’s fast, has a good API, and can replace Jasmine/Karma for most scenarios. I like Cypress for end-to-end testing since it offers a more reliable browser environment than Protractor, especially when testing asynchronous flows like logins. For complex services, I mock dependencies with Angular’s TestBed and use spies to track function calls and observables.
Can you walk me through how you handle lazy loading in Angular modules and its impact on application performance?

I break the app into distinct feature modules and configure the router to lazy load those modules only when the user navigates to them. This reduces the initial load time by only loading what's needed at startup. I also use route preloading to asynchronously load modules in the background after the main content is rendered, which improves the user experience by reducing the wait time when navigating to those routes.
What are your best practices for Angular component communication, especially between deeply nested components?

For parent-child communication, I use @Input() and @Output() bindings, which are straightforward. For sibling or distant component communication, I avoid “prop drilling” and instead use shared services with RxJS subjects or NgRx if there’s a need for more complex state management. This keeps the communication clean and reduces tight coupling between components.
How do you manage upgrades in Angular, especially when major versions introduce breaking changes?

I start by reviewing the Angular changelogs to identify breaking changes. For each version update, I use ng update to handle package migrations and take care of any manual steps for major upgrades. I also use the Angular Update Guide to ensure that my code is compliant with the latest API changes. Automated testing and linting help catch issues during the upgrade process.
How do you implement security best practices in Angular applications, particularly around XSS and authentication?

Angular provides built-in XSS protection with its template engine, but I ensure I never bypass security contexts or use innerHTML directly. For authentication, I implement JWT or OAuth2 for securing API calls, and I use HttpInterceptors to attach tokens and handle errors globally. I also ensure that route guards (CanActivate) are in place to protect restricted areas of the app.
What tools or libraries do you commonly use to enhance the development experience when working with Angular, such as for state management, animations, or UI components?

For state management, I use NgRx or Akita depending on the application size. I rely on Angular Material for a consistent UI component library, and I use ngx-translate for internationalization. For animations, I use Angular’s built-in animations API because it integrates well with change detection. I also use tools like ESLint for code quality and Prettier for consistent formatting across the team.
