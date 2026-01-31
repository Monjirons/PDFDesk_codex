# Implementation Plan - Phase 1: PDF Import & Material Shelf

## Goal Description

Enable users to import PDF files (via Drag & Drop or File Menu) and manage them in the Bottom Panel (Material Shelf). The imported PDFs will be displayed as "cover cards" in a horizontal scrollable list. Users can reorder these cards (visual only) and remove them.

This phase focuses on the **Input** side of the application workflow.

## User Review Required
>
> [!NOTE]
> **PDF Parsing Strategy**: For this phase, we will implement a basic PDF loader using `pdfjs-dist` to extract:
>
> 1. File Name
> 2. Total Page Count
> 3. First Page Thumbnail (for the cover card)
>
> We will not yet implement the full page text extraction or high-res rendering required for the editing view.

## Proposed Changes

### [Store Updates]

#### [MODIFY] `src/store/appStore.ts`

- Update `PDFDocument` interface to include real data properties (`file`, `thumbnailUrl`, `id`).
- Add actions:
  - `addSourcePDF(file: File)`: Async action to process file and add to store.
  - `removeSourcePDF(id: string)`: Remove from `sourcePDFs` list.
  - `reorderSourcePDFs(startIndex: number, endIndex: number)`: Reorder logic.

### [New Components]

#### [NEW] `src/utils/pdfLoader.ts`

- Utility functions to interface with `pdfjs-dist`.
- `loadPDF(file: File)`: Returns metadata and cover image.

#### [NEW] `src/components/features/PDFCard.tsx`

- UI component for a single PDF in the shelf.
- Displays: Thumbnail, Filename, Page count.
- Handles: Selection click, Right-click context menu (Remove), Drag handles.

#### [NEW] `src/components/features/MaterialShelf.tsx`

- Container for `PDFCard` list.
- Implements:
  - Horizontal scrolling.
  - Drag & Drop sorting (using `@dnd-kit/core` or similar lightweight logic if complex dnd is overkill, but `dnd-kit` is recommended for robust interactions). **Decision**: Will start with simple HTML5 DnD or a lightweight library `dnd-kit` if allowed. *Update: I will use `@dnd-kit/core` and `@dnd-kit/sortable` for robust sortable lists.*

#### [NEW] `src/components/features/FileDropZone.tsx`

- A transparent overlay or event listener on the main app container to handle file drops.

### [UI Integration]

#### [MODIFY] `src/App.tsx`

- Integrate `FileDropZone` at the root.
- Replace placeholder in `BottomPanel` with `MaterialShelf`.

## Verification Plan

### Automated Tests

- Unit test for `reorderSourcePDFs` in store (logic verification).

### Manual Verification

- **Import**:
  - Drag & drop a PDF file into the window -> Check if it appears in the bottom shelf.
  - Try dragging non-PDF files -> Should be ignored or show error.
- **Display**:
  - Verify the card shows the correct filename and a thumbnail (or loading placeholder).
- **Manage**:
  - Click a card -> Selected state looks active (visual only for now).
  - Drag card A to position B -> Order changes.
  - Right-click/Delete button -> Card disappears.
