# Phase 10: Right Panel Drag & Drop Implementation Plan

## Goal

Enable dragging pages from the Right Panel (Material Shelf) directly onto the Infinite Canvas to add them to the workspace at the specific drop location.

## User Review Required
>
> [!IMPORTANT]
> This change requires significant refactoring of the Drag & Drop architecture (`DndContext` hoisting).
> The `InfiniteCanvas` will no longer manage its own Drag Context, meaning `App.tsx` will become the central handler for all drag operations.

## Proposed Changes

### Component Architecture

Current:

- `InfiniteCanvas` (DndContext) -> internal items only
- `MaterialPageList` (No DnD)

New:

- `App.tsx` (DndContext)
  - `MaterialPageList` (Draggable Sources)
  - `InfiniteCanvas` (Droppable Target & Existing Items)

### File Changes

#### [MODIFY] [App.tsx](file:///c:/Users/meruy/.gemini/PDFDesk/src/App.tsx)

- Wrap `app-body` with `DndContext`.
- Implement centralized `handleDragEnd`.
- Manage `DragOverlay` to show the thumbnail while dragging across panels.

#### [MODIFY] [InfiniteCanvas.tsx](file:///c:/Users/meruy/.gemini/PDFDesk/src/components/features/InfiniteCanvas/InfiniteCanvas.tsx)

- Remove `DndContext`.
- Accept `ref` or expose `TransformWrapper` instance to allow `App.tsx` to calculate drop coordinates.
- Ensure it acts as a `Droppable` zone (`id: 'canvas-drop-zone'`).

#### [MODIFY] [MaterialPageList.tsx](file:///c:/Users/meruy/.gemini/PDFDesk/src/components/features/MaterialPageList.tsx)

- Wrap thumbnails in a new `DraggableThumbnail` component.
- Set drag data: `{ type: 'new-page', pdfId, pageNumber }`.

### Coordinate Calculation Logic

When dropping a 'new-page' item:

1. Get drop coordinates (Screen X/Y) from `DragEndEvent`.
2. Get Canvas Transform (Pan X/Y, Scale) from `TransformWrapper` ref.
3. Calculate:
   `CanvasX = (ScreenX - PanX - CanvasOffsetLeft) / Scale`
   `CanvasY = (ScreenY - PanY - CanvasOffsetTop) / Scale`
4. Call `addPageToTarget(..., CanvasX, CanvasY)`.

## Verification Plan

### Manual Verification

1. **Right Panel Drag**: Drag a page from the Right Panel. Verify `DragOverlay` follows mouse.
2. **Drop on Canvas**: Drop onto the canvas. Verify page appears at the exact mouse location.
3. **Drop outside**: Drop on UI panels. Verify nothing happens.
4. **Existing Functionality**: Verify existing Canvas dragging and Background dragging still work (Regression Test).
