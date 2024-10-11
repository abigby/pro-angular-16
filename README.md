# Manning Source Code

This repository accompanies *Pro Angular 16* by Adam Freeman.

Download the files as a zip using the green button or clone the repository to your machine using Git.

## Releases

This repository corresponds to the examples in the published book, without corrections or updates. See [this](errata.md) file for serious issues that are likely to prevent the examples from working as described in the book. See [this](typos.md) file for small mistakes that I intend to correct in the next edition.

## Contributions

If you find an error, please contact the author using the email address published in the book. Please do not create pull requests because they will not be accepted. Please do not create issues because the author will not respond to them.


Angular 18:
What are the key changes introduced in Angular 18, and how do they impact the development of enterprise applications?

Angular 18 has made several improvements, particularly in performance and tooling. One of the key changes is the enhanced tree-shaking, which helps reduce bundle sizes by eliminating unused code. There’s also improved error messages and type-checking in templates, making debugging easier. The stricter typing, especially for forms and reactive services, improves the robustness of enterprise applications, helping to catch issues earlier in the development cycle.
How do you approach migrating an existing Angular application to Angular 18? What are the critical steps or challenges you typically encounter?

I start by reading the release notes to understand breaking changes, then use ng update to automatically handle dependencies. After updating, I ensure all libraries are compatible, especially third-party ones like AG-Grid or Angular Material. Running the app and its tests in a staging environment helps identify any regressions or issues with new APIs. A common challenge is fixing deprecations or breaking changes in Angular’s forms and routing APIs.
Have you noticed any performance improvements with Angular 18, particularly with rendering or change detection? Can you explain how they work?

Yes, Angular 18’s change detection has improved thanks to enhanced optimizations around tracking dirty components. Additionally, tree-shaking and bundle size optimizations mean that the application loads faster. The improvements in Angular’s internal rendering pipeline reduce the number of change detection cycles, leading to smoother performance, especially in applications with complex UIs or high user interaction.
Angular 18 introduced new features for the Angular CDK. Can you explain how you’ve leveraged the CDK in your projects, particularly for building reusable components?

The Angular CDK is invaluable for building reusable components. I use the CDK’s new drag-and-drop module for custom sortable lists and tables. The CDK also offers improved utilities for accessibility, focus management, and overlays, which make it easier to build components that integrate well into the overall UX. Angular 18’s CDK improvements simplify component composition, making custom tooltips, dialogs, and data tables more manageable.
Angular Material M3 Theming:
How do you customize the M3 theme in Angular Material 18 to match specific branding requirements? Can you walk through the process of creating custom palettes?

To customize the M3 theme, I start by generating custom palettes using Angular Material’s theme generator tools. I define primary, accent, and warn color palettes based on the brand’s color scheme. I then apply these palettes to the theme using the @use Sass API to override default M3 tokens. This approach allows me to create themes that align with the client’s branding while still adhering to the Material 3 design principles. For advanced customization, I tweak typography and component-level styles within the theme.
Can you explain how Angular Material's new M3 theming system differs from M2 and how it impacts the design and development of a UI?

The M3 system is more dynamic compared to M2, focusing on personalization and expressive design elements like rounded corners and large touch targets. Material 3 introduces a more flexible color system with tonal palettes, allowing for greater adaptability between light and dark modes. It also supports dynamic color schemes, meaning the UI can adapt to the system theme more smoothly. This impacts development by requiring designers and developers to account for dynamic colors and varying UI densities.
How do you handle theming across large applications that require multiple color schemes (e.g., light/dark modes) using Angular Material's M3 system?

I leverage Angular Material’s built-in support for light and dark themes by creating two distinct theme files and switching between them using a global service. I use CSS variables to dynamically change theme settings without reloading the app. Angular Material M3’s tonal color palettes make it easier to adapt components to light or dark backgrounds. In a large app, I ensure that key UI components such as forms, dialogs, and navigation elements are tested thoroughly in both modes for consistency.
What challenges have you faced when integrating Material M3 theming into existing Angular applications, and how did you resolve them?

One challenge was adapting the new M3 component design system to an existing M2-based design. Components look and behave slightly differently in M3, especially with the new emphasis on large touch targets and rounded corners. To resolve this, I created a migration plan to slowly upgrade components, starting with shared ones, and gradually updated styles across the app. Another challenge was ensuring compatibility with custom components, which required adjustments to spacing, typography, and density settings.
AG-Grids:
What is your approach to integrating AG-Grids with Angular, particularly in complex applications where data updates frequently?

I use AG-Grid’s reactive data model to handle frequent data updates, either by using the ImmutableData property for high-frequency updates or leveraging the Angular async pipe to streamline the rendering process. I prefer the server-side row model for better scalability in applications dealing with large datasets, and I ensure that change detection is optimized by using Angular’s OnPush strategy where necessary.
How do you optimize performance in AG-Grids when handling large datasets or complex cell rendering in Angular applications?

To optimize performance, I rely on AG-Grid’s virtualization features, such as infinite scrolling and lazy loading, for large datasets. I also implement cell renderers efficiently by offloading complex logic to services and avoiding unnecessary DOM updates. For complex grids, I use the valueGetter and cellRenderer properties wisely to minimize overhead. Server-side pagination and chunking of data loads further improve performance when dealing with massive datasets.
How do you customize AG-Grid’s cell templates and column behavior in conjunction with Angular Material components?

I create custom cell renderers that integrate Angular Material components, like mat-checkbox or mat-icon, for more interactive and responsive grid cells. I use Angular’s component factories for dynamic component rendering in grid cells. This allows me to inject Material components into AG-Grid seamlessly while maintaining Angular’s data binding and reactive forms capabilities.
Have you used AG-Grid’s server-side row model in Angular? How do you handle server-side pagination and filtering efficiently in your applications?

Yes, I use the server-side row model to handle large datasets. I implement server-side pagination by passing the pagination state to the backend, which returns only the relevant data for the current page. For filtering, I use the AG-Grid event hooks to send filter data to the server, ensuring the grid remains responsive. I also optimize the backend queries to ensure fast response times, especially when multiple filters are applied.
These responses reflect in-depth knowledge of Angular 18, Angular Material M3 theming, and AG-Grids, showcasing a well-rounded understanding of how to handle complex scenarios in enterprise applications.
