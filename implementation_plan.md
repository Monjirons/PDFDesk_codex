# Implementation Plan - Phase 11: Asymmetric Split View (Strict Spec)

## Goal

Implement the **Vertical Split View Architecture** defined in `PDFDesk_SplitView_Spec.md`.
Top: **Infinite Canvas** (Thinking Surface, Non-destructive).
Bottom: **Preview View** (Source of Truth, Linear Order).

## User Review Required
>
> [!IMPORTANT]
> **Architecture Shift**: This is NOT just two views of the same thing.
>
> - **Top View** is the `InfiniteCanvas`.
> - **Bottom View** is a NEW `PreviewPageList` (Linear, Scrollable).
> - **Save Button** only respects the Preview View order.

## Proposed Changes

### 1. Store (`src/store/appStore.ts`)

#### [MODIFY] `UIState`

- Add `isSplitMode: boolean` (Default: `false`? Or `true` based on spec "Core Concept"? Spec says "uses two asymmetric views", implying this might be the *default* or *only* view mode eventually. For now, we will add it as a toggleable mode `splitView` in `ViewMode` or a separate flag).
  - *Decision*: Keep `ViewMode` string, add `'splitView'` as a new mode.
- Add `splitRatio: number` (Default 0.5).

#### [MODIFY] `PDFState` (Sync Logic)

- **Selection Sync**:
  - `selectPage(id)` needs to trigger scroll in Preview View.
  - We might need an ephemeral `previewScrollTargetId` in store to signal the view to scroll.

### 2. New Component: `PreviewPageList` (Bottom View)

#### [NEW] `src/components/features/PreviewPageList.tsx`

- **Purpose**: "Canonical / authoritative" view.
- **UI**: Vertical list of pages (like standard PDF reader).
- **Data**: `leftPages` sorted by `pageNumber`?
  - *Wait**: `leftPages` currently holds `x,y`. Does it hold the *order*?
  - *Correction*: `leftPages` is the "Target" (Edited) PDF. The spec says "Preview View reflects Real PDF page order".
  - Currently, `leftPages` *is* the edited state.
  - **Crucial**: The Infinite Canvas allows "Moving freely" WITHOUT changing PDF order.
  - **Conflict**: Currently `leftPages` stores BOTH position (x,y) AND order (array index).
  - **Resolution**:
    - `APP_STORE` modification needed?
    - If moving on Canvas *doesn't* change order, then `x,y` changes must be decoupled from `leftPages` array index order.
    - *Current App Logic*: `updatePagePositions` changes `x,y`. `movePageInTarget` changes Array Order.
    - *Strict Spec*: "Rearranging pages on the canvas [Top] ... does NOT change PDF order [Bottom]".
    - **Implication**: Dragging on Canvas calls `updatePagePositions` (x,y only). Dragging on Preview (if allowed) would call `movePageInTarget` (Order).
    - **Refinement**: The "Preview View" must display `leftPages` in their *Array Order*. The "Canvas View" displays `leftPages` at their `x,y`.

### 3. Layout Implementation (`src/App.tsx`)

#### [MODIFY] Structure

- If `viewMode === 'splitView'`:
  - **Top Pane**: `InfiniteCanvas` (Height: `splitRatio`)
  - **Resizer**: (Horizontal bar)
  - **Bottom Pane**: `PreviewPageList` (Height: `Remaining`)

### 4. Synchronization Logic (`src/App.tsx` or Store)

#### [MODIFY] `handleDragEnd` (Canvas)

- **Top Canvas Drop**:
  - Updates `x,y` (`updatePagePositions`).
  - **DOES NOT** call `movePageInTarget` (Order change).
- **Bottom Preview Drop** (If reordering is allowed in Preview? Spec says "User edits PDF order via dedicated editing tools (outside canvas) 2. Preview reflects true page order". It implies Preview *is* where you see the order, maybe edit it too? Spec is silent on *dragging inside preview*, but implies that's where "truth" is. I will assume Preview is Read-Only for *layout* but reflects the `leftPages` order. **Wait, Flow C says "User edits PDF order via dedicated editing tools (outside canvas)"**.
  - *Interpretation*: The existing "Left Panel" (MaterialPageList/TargetPageList) is likely the "dedicated editing tool".
  - *Correction*: The Preview View is strictly for *Verification*.
  - *Refined Plan*: Preview View simply renders `leftPages` in order.

### 5. Sync Implementation

- **Scroll Sync**:
  - When `selectedPageIds` changes (single select):
  - `PreviewPageList` useEffect detects change -> finds DOM element -> `scrollIntoView`.

## Verification Plan

1. **Visual Split**: Verify Top (Canvas) and Bottom (Preview) appear.
2. **Asymmetry**:
   - Move page in Top (Canvas). Verify Bottom (Preview) order *does not change*.
   - Reorder page in Left Panel (Target List). Verify Bottom (Preview) order *changes*.
3. **Selection Sync**:
   - Click page in Top. Verify Bottom scrolls to it.
   - Click page in Bottom. Verify Top highlights it.
4. **Data Integrity**:
   - Save PDF. Verify the output matches Bottom View order, NOT Top Canvas visual layout.
