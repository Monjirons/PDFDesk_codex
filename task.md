# PDFDesk Task List

## Phase 0: Project Backbone & UI Skeleton
- [ ] **UI Layout Implementation**
    - [ ] Create main layout structure (Orange/Purple/White/Left/Right/Bottom panels)
    - [ ] Implement panel toggles (Show/Hide)
    - [ ] Implement ViewMode switching (PageView / InfiniteCanvas placeholders)
- [ ] **State Model Scaffolding**
    - [ ] Define `ActiveEditPDF`
    - [ ] Define `SourcePDFs`
    - [ ] Define `SelectedSourcePDF`
    - [ ] Define `LeftPages`
    - [ ] Define `HiddenFlags`

## Phase 1: PDF Import & Material Shelf (Bottom)
- [ ] **PDF Loading**
    - [ ] File menu implementation
    - [ ] Drag & Drop to app
- [ ] **Bottom Panel (Material Shelf)**
    - [ ] Display cover cards (horizontal scroll)
    - [ ] Drag & Drop reordering (visual only)
    - [ ] Remove PDF (Right click/Del)

## Phase 2: Material Pages (Right) & Selection Sunc
- [ ] **Selection Logic**
    - [ ] Select `SelectedSourcePDF` in bottom panel
- [ ] **Right Panel**
    - [ ] Display page thumbnails of selected PDF
    - [ ] Implement open/close behavior (Auto-open logic)

## Phase 3: Editing Target (Left) & Page Addition (Right -> Center -> Left)
- [ ] **Editing Target**
    - [ ] Manage `ActiveEditPDF`
- [ ] **Page Addition**
    - [ ] D&D from Right to Center adds to Left (append)
- [ ] **Left Panel Operations**
    - [ ] Reorder pages (D&D)
    - [ ] Visibility Toggle (Eye icon)

## Phase 4: Export & Visibility Handling
- [ ] **Export Logic**
    - [ ] Output pages in Left Panel order
    - [ ] Exclude hidden pages
- [ ] **MVP Verification**

## Phase 5: Undo/Redo
- [ ] **Undo Logic**
    - [ ] Implement Undo/Redo for critical actions (Add/Delete/Reorder/Hide/Group)

## Phase 6: Infinite Canvas
- [ ] **Basic Display**
    - [ ] Place PageObjects on canvas
- [ ] **Collision Resolution**
    - [ ] Improve placement logic (no overlap)

## Phase 7: Panel Resize & Persistence
- [ ] **Resizing**
    - [ ] Implement splitters for Left/Right/Bottom
- [ ] **Persistence**
    - [ ] Save/Restore panel sizes

## Phase 8: Grouping & Batch Visibility
- [ ] **Grouping**
    - [ ] Implement Group/Ungroup
    - [ ] Batch visibility toggle

## Phase 9: Split View (Future)
- [ ] **Split Mode**
    - [ ] Implement Upper/Lower split view
