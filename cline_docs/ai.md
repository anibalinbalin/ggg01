Okay, let's break down the Midday.ai codebase and extract the key components you're interested in – customer creation/editing, the calendar system, the sidebar, and overall styling – into a comprehensive Product Requirements Document (PRD) suitable for your new project.

Project: Feature Extraction from Midday.ai (for New Project)

Date: 2025-01-15

Version: 1.0

1. Introduction

This PRD outlines the requirements for extracting and adapting specific features from the Midday.ai open-source project (located at the provided GitHub repository) for use in a new project. The focus areas are:

Customer Management: The ability to create, edit, and display customer information, mimicking Midday's customer interface.

Calendar System: A calendar component similar to Midday's tracker, with features for date selection, event display, and potentially scheduling.

Sidebar: A sidebar navigation component with similar styling and behavior to Midday's.

Styling: Replicating the core visual style (colors, typography, spacing) of Midday.ai, particularly as implemented with Tailwind CSS.

This document will detail the functional and non-functional requirements for each feature, referencing the relevant Midday.ai code files where applicable. It will also outline considerations for adapting these features to a new codebase.

2. Overall System Design Considerations

Before diving into specific features, here are some overarching considerations drawn from the Midday.ai codebase:

Technology Stack: The new project should strongly consider using Next.js (App Router), React, TypeScript, Tailwind CSS, and Radix UI (or Shadcn/ui, which builds on Radix). These technologies are heavily used in Midday.ai and provide the desired UI/UX.

State Management: Zustand is used in several places (e.g., store/user/hook.ts, store/vault/hook.ts, store/assistant.ts). Consider a similar lightweight state management solution.

Server Actions: Next.js Server Actions are used extensively. The new project should leverage this pattern for data fetching and mutations.

Data Fetching: Supabase is the primary database and backend service. Consider using Supabase, or a similar service that provides real-time capabilities.

Utility Functions The package utils contains many usefull functions that will help a lot.

Component Structure: Midday.ai uses a well-organized component structure. The new project should follow a similar pattern, separating concerns into components, hooks, actions, and utilities.

Internationalization the app uses translations.

3. Feature Requirements
3.1 Customer Management

3.1.1 Functional Requirements

Create Customer:

The user should be able to create a new customer with the following fields:

Name (required, text input)

Email (required, email input)

Phone (optional, text input)

Website (optional, text input, potentially auto-formatted)

Contact Person (optional, text input)

Address (Line 1, Line 2, City, State, Zip, Country) - Consider using a structured address component or API (like the Google Places API integration used in Midday.ai for search-address-input.tsx).

VAT Number (optional, text input, potentially validated based on country)

Notes (optional, textarea)

Tags (optional, multiple select/tag input, see select-tags.tsx)

Form validation should be implemented (using Zod as in src/actions/schema.ts is recommended).

Upon successful creation, the user should be notified (e.g., using a toast, as seen with useToast).

Data should be persisted to a database (consider Supabase, as used by Midday.ai).

Server actions should handle the creation (see src/actions/customer/create-customer-action.ts).

Edit Customer:

Users should be able to edit all fields available during creation.

The form should pre-populate with existing customer data.

Updates should be persisted to the database.

Server actions should handle the update.

Delete Customer:

Users should be able to delete a customer.

A confirmation dialog should be presented before deletion (as seen with AlertDialog in Midday).

Deletion should be handled via a server action.

List/Display Customers:

Customers should be displayed in a table format (see src/components/tables/customers).

Columns should include: Name, Contact Person, Email, potentially "Invoices" and "Projects" (if those features are relevant to the new project), and an "Actions" column.

The table should support sorting (at least by Name, as in Midday's TableHeader component).

The table should support filtering/searching (as seen in CustomersHeader).

Consider pagination or infinite scrolling if a large number of customers is expected.

Tagging:

Customers should be taggable, allowing for categorization (see src/components/select-tags.tsx for implementation).

Tags should be creatable and deletable.

Customers can have multiple tags.

3.1.2 Non-Functional Requirements

Responsiveness: The customer management interface should be responsive and work well on various screen sizes.

Accessibility: Adhere to accessibility best practices (WCAG).

Performance: The customer list should load and filter quickly, even with a large number of customers.

Error Handling: Provide clear error messages to the user if any operation fails.

Security: Ensure data is protected according to best practices (e.g., using Supabase's Row Level Security, if applicable).

3.1.3 Relevant Midday.ai Files

src/actions/customer: Server actions for customer creation, deletion, and tag management.

src/actions/schema.ts: Zod schemas for validation.

src/app/[locale]/(app)/(sidebar)/customers/page.tsx: Customer listing page.

src/components/tables/customers: Table components for displaying customers.

src/components/forms/customer-form.tsx: The customer form.

src/components/open-customer-sheet.tsx: The sheet/dialog trigger.

src/components/sheets/customer-create-sheet.tsx: The sheet for creating customers.

src/components/sheets/customer-edit-sheet.tsx: The sheet for editing customers.

src/components/search-address-input.tsx: Address input with auto-suggestions.

src/components/select-tags.tsx: Tag selection component.

src/components/vat-number-input.tsx: VAT number input with validation.

src/hooks/use-customer-params.ts: A Hook for managing customer related URL parameters.

3.2 Calendar System

3.2.1 Functional Requirements

Display:

Display a monthly calendar view.

Show events/entries on their respective dates.

Visually distinguish between days with and without entries.

Allow navigation between months (previous/next).

Highlight the current day.

Optionally support a week start day setting (Monday/Sunday).

Event/Entry Display:

Show a summary of events for a day (e.g., total duration, project names).

Provide a detailed view of events for a selected day (e.g., in a sidebar or modal).

Visually differentiate events by project (e.g., using colors).

Event/Entry Creation/Editing:

Allow users to create new events/entries by clicking on a day.

Support editing existing events/entries.

Fields for events should include:

Start time

End time

Project selection (with a dropdown/search)

Description (optional)

Assigned user (optional)

Duration (automatically calculated based on start/end time)

Drag and Drop (Optional):

Allow users to drag and drop events to adjust their time.

Allow users to resize events to adjust their duration.

3.2.2 Non-Functional Requirements

Performance: The calendar should load quickly and respond smoothly to user interactions.

Responsiveness: The calendar should adapt to different screen sizes.

Accessibility: Adhere to accessibility best practices (e.g., keyboard navigation).

3.2.3 Relevant Midday.ai Files

src/components/tracker-calendar.tsx: Main calendar component.

src/components/tracker-day-select.tsx: Component for selecting a day.

src/components/tracker-entries-list.tsx: Component for listing entries for a day.

src/components/tracker-events.tsx: Component for rendering events on the calendar.

src/components/tracker-month-select.tsx: Component for selecting a month.

src/components/forms/tracker-record-form.tsx: Form for creating/editing tracker entries.

src/components/forms/update-record-form.tsx: Form for updating a record.

src/components/open-tracker-sheet.tsx: Sheet/dialog trigger for tracker.

src/components/sheets/tracker-sheets.server.tsx: Sheet for tracker.

src/components/tables/tracker: Various tracker table components.

src/hooks/use-tracker-params.ts: Hook for managing tracker related URL parameters.

src/utils/tracker.ts: Utility functions for date calculations and transformations.

src/actions/project: Actions related to project, required for creating tracker entries

3.3 Sidebar

3.3.1 Functional Requirements

Navigation: Provide primary navigation links to main sections of the application.

Responsiveness:

On desktop, the sidebar should be visible and fixed.

On mobile, the sidebar should be hidden by default and accessible via a toggle button (hamburger menu).

Customization: Allow users to customize the order of sidebar items (drag-and-drop). This is optional, but a nice-to-have.

Icons: Each sidebar item should have an associated icon.

Active State: The currently active page should be visually highlighted in the sidebar.

Team/Account Switching: Include a dropdown or similar mechanism for switching between teams/accounts (if your project supports multiple teams/accounts ).

Expandable/Collapsible: Allow users to expand and collapse sections of the sidebar.

3.3.2 Non-Functional Requirements

Accessibility: Ensure proper ARIA attributes and keyboard navigation.

Performance: Sidebar should load and render quickly.

Consistency: Maintain consistent styling and behavior with the rest of the application.

3.3.3 Relevant Midday.ai Files

src/components/sidebar.tsx: Main sidebar component.

src/components/mobile-menu.tsx: Mobile menu implementation.

src/components/main-menu.tsx: Menu items within the sidebar.

src/components/team-menu.tsx: Team selection dropdown (if applicable).

src/components/team-dropdown.tsx: Team selection dropdown (if applicable).

src/store/menu.ts: Zustand store for managing menu state (customization).

3.4 Styling

3.4.1 General Approach

Midday.ai uses Tailwind CSS for styling. The new project should adopt Tailwind CSS as well. The tailwind.config.ts file in the packages/ui directory provides the base configuration.

3.4.2 Colors

Extract the color palette from tailwind.config.ts and src/styles/globals.css. Key colors to note:

primary: Used for main actions and highlight colors.

secondary: Used for secondary actions and elements.

background: Background color of the application.

foreground: Text color on the background.

border: Color for borders.

destructive: Color for destructive actions (e.g., delete).

muted: Color for muted text and backgrounds.

accent: Color for accents.

3.4.3 Typography

Midday.ai uses the GeistSans and GeistMono fonts. Include these fonts in your project. The globals.css file shows how these are included.

Note the use of font-mono for code-like elements and font-medium for certain headings and text.

3.4.4 Spacing and Layout

Midday.ai uses a consistent spacing scale, likely based on Tailwind's default spacing scale (multiples of 4px). Pay attention to margins, padding, and gaps used throughout the components.

The layout often uses flex and grid for responsive design.

3.4.5 Component-Specific Styles

Many components have specific class names that apply styles. Examine the components in packages/ui/src/components and note the use of the cn utility (from packages/ui/src/utils) to combine Tailwind classes.

Pay attention to hover states, focus states, and disabled states, all of which are styled using Tailwind utility classes.

3.4.6 Dark Mode

Midday.ai supports dark mode using the dark: variant in Tailwind classes. Ensure your components handle dark mode correctly, referencing Midday's implementation as a guide.

3.4.7 Relevant Midday.ai Files

packages/ui/tailwind.config.ts: Base Tailwind configuration.

apps/dashboard/src/styles/globals.css: Global styles, including CSS variables for colors.

packages/ui/src/components: Examine individual components for their specific styling.

4. Implementation Notes

Component Reuse: You can directly copy and adapt many of the UI components from packages/ui/src/components. You may need to adjust imports to match your project structure.

Server Actions: Adapt the server actions from apps/dashboard/src/actions to your backend logic. If you're not using Supabase, you'll need to replace the Supabase client calls with your chosen data fetching mechanism.

Context and State: Midday.ai uses context providers (e.g., UserProvider) and Zustand stores. Replicate this pattern as needed for your application state.

Error Handling: Midday.ai uses safe-action which integrates nicely with zod for type checking and error handling.

5. Future Considerations

Error Boundaries: Ensure consistent error handling, use next-safe-actions.

Testing: Midday.ai has some basic tests, but a new project should have a comprehensive testing strategy.

Performance: Pay attention to performance, especially with data loading and large lists (consider virtualization if needed).

Accessibility: Ensure all components are accessible (keyboard navigation, ARIA attributes).

This PRD provides a starting point for extracting the desired features from Midday.ai. You'll need to adapt the code to fit your specific project structure and requirements, but this document should give you a solid foundation and point you to the relevant code sections. Remember to focus on understanding how Midday.ai implements each feature, rather than just copying code verbatim. Good luck!