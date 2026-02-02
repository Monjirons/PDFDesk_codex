# PDFDesk Task List

## Phase 0: Project Backbone & UI Skeleton

- [x] **UI Layout Implementation**
  - [x] Create main layout structure
  - [x] Implement panel toggles
  - [x] Implement ViewMode switching
- [x] **State Model Scaffolding**
  - [x] Define Store Slices (`uiState`, `pdfState`)

## Phase 1: PDF Import & Material Shelf (Bottom)

- [x] **PDF Loading**
  - [x] File menu & Drag/Drop Import
  - [x] Loading states
- [x] **Bottom Panel (Material Shelf)**
  - [x] Display cards & Reorder
  - [x] Remove PDF

## Phase 2: Material Pages (Right) & Selection Sync

- [x] **Selection Logic**
  - [x] Store update: `selectedSourceId`
- [x] **Right Panel (Material Pages)**
  - [x] Display individual pages (Draggable)
  - [x] Thumbnail generation

## Phase 3: Editing Target (Left) & Page Addition

- [x] **Editing Target**
  - [x] Manage `ActiveEditPDF`
- [x] **Page Addition**
  - [x] D&D from Right to Canvas/Target
- [x] **Left Panel Operations**
  - [x] Reorder pages
  - [x] Visibility Toggle

## Phase 4: Export

- [x] **Export Logic**
  - [x] Export visible pages in order

## Phase 5: Undo/Redo

- [x] **Undo Logic**
  - [x] Integrated `zundo` with Store

## Phase 6: Infinite Canvas

- [x] **Basic Display**
  - [x] Render `CanvasItem` at coordinates
  - [x] Zoom/Pan support (`react-zoom-pan-pinch`)
- [x] **Drag & Drop**
  - [x] Canvas Item Pulling (From List)
  - [x] Background Grid Extraction
  - [x] Canvas Item Moving
- [x] **Collision Resolution**
  - [x] Basic overlap prevention (Push logic)

## Phase 7: Panel Resize

- [x] **Resizing**
  - [x] Splitters for panels

## Phase 9: Persistence

- [x] **IndexedDB Implementation**
  - [x] Project state saving/loading

## Phase 8: Grouping & Advanced Selection

- [x] **Multi-Selection Handling**
  - [x] Implement Multi-Item Drag
  - [x] Visual feedback for selection group
- [x] **Grouping Logic**
  - [x] Group/Ungroup Actions

## Phase 11: Asymmetric Split View (Strict Spec)

- [ ] **State & UI**
  - [ ] Add `splitView` to `ViewMode`
  - [ ] Add `splitRatio` (Default 0.5)
- [ ] **Components**
  - [ ] Create `PreviewPageList` (Bottom View - Canonical)
  - [ ] Implement Vertical Split Layout in `App.tsx`
- [ ] **Sync Logic**
  - [ ] Canvas Selection -> Preview Scroll
  - [ ] Preview Selection -> Canvas Highlight
  - [ ] Ensure Canvas Move != Preview Reorder

## Future Phases

- [ ] **Alignment Tools** (Snap to grid, Align Left/Right)
