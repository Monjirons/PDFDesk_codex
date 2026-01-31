# Implementation Plan - Phase 0: Project Backbone & UI Skeleton

## Goal Description

Establish the UI structure and state management foundation for PDFDesk. This phase focuses on creating the static layout with panel toggles and identifying the core state models, ensuring the application "shell" is ready for future feature additions without architectural restructuring.

## User Review Required

None for this phase.

## Proposed Changes

### Tech Stack & Project Structure

- **Framework**: React + TypeScript (assumed standard for this type of app, or Vanilla as per `web_application_development` rules? The user didn't specify, but `web_application_development` says "If the USER specifies that they want a more complex web app, use a framework like Next.js or Vite". Since this is an "app" (PDFDesk) and likely needs complex state, I will use **Vite + React + TypeScript**.
- **Styling**: Vanilla CSS (CSS Modules or global CSS) as per rules. avoiding Tailwind unless requested.
- **State Management**: Zustand or Context API? Given the complex state (PDFs, pages, selection), **Zustand** is recommended for better performance and separation of concerns.

### [UI Layout]

#### [NEW] `src/App.tsx`

- Main layout container (Grid/Flex).
- Components for:
  - `Header` (Orange)
  - `QuickToolbar` (Purple)
  - `MainView` (White - Placeholder)
  - `LeftPanel` (Page List)
  - `RightPanel` (Material Pages)
  - `BottomPanel` (Material Shelf)

#### [NEW] `src/components/layout/Panel.tsx`

- Generic collapsible panel component.

### [State Management]

#### [NEW] `src/store/appStore.ts`

- Define interfaces:
  - `PDFDocument` (source)
  - `PageObject`
- State slices:
  - `uiState`: panel visibility (`leftOpen`, `rightOpen`, `bottomOpen`), view mode (`pageView` vs `infiniteCanvas`).
  - `pdfState`: `sourcePDFs`, `activeEditPDF`, `selectedSourceId`, `selectedPageId`.

## Verification Plan

### Automated Tests

- Check if the app builds and runs.
- Verify state updates (console logs or devtools) when toggling panels.

### Manual Verification

- **Launch App**: Verify the layout matches the wireframe keywords (Orange/Purple/White structure).
- **Panel Toggles**: Click buttons to open/close Left/Right/Bottom panels.
- **Resize Check**: Ensure UI doesn't break when window is resized (though panel resizing itself is Phase 7).
