---
title: Migrate Upload with progress
impact: MEDIUM
impactDescription: Replace Ant Upload with accessible file input/dropzone + progress
tags: components, upload, forms
---

## Migrate Upload with progress

Replace Ant `Upload`/`Dragger` with a file input + dropzone pattern and explicit progress reporting.

### Pattern

```tsx
'use client'
import { useState } from 'react'
import { Progress } from '@/components/ui/progress'
import { Button } from '@/components/ui/button'

export function FileUpload({ onUpload }: { onUpload: (files: File[]) => Promise<void> }) {
  const [progress, setProgress] = useState(0)
  const [loading, setLoading] = useState(false)

  const handleFiles = async (files: FileList | null) => {
    if (!files?.length) return
    setLoading(true)
    setProgress(5)
    await onUpload(Array.from(files))
    setProgress(100)
    setLoading(false)
  }

  return (
    <div className="space-y-3">
      <div
        className="border border-dashed rounded-lg p-4 text-sm text-muted-foreground"
        onDrop={e => { e.preventDefault(); handleFiles(e.dataTransfer.files) }}
        onDragOver={e => e.preventDefault()}
      >
        <input type="file" multiple onChange={e => handleFiles(e.target.files)} aria-label="Upload files" />
        <p className="mt-2 text-xs text-muted-foreground">Drag and drop or click to select. Max 10MB each.</p>
      </div>
      {loading && <Progress value={progress} />}
      <div className="flex gap-2">
        <Button type="button" onClick={() => handleFiles((document.querySelector('input[type=file]') as HTMLInputElement)?.files)} disabled={loading}>Upload</Button>
      </div>
    </div>
  )
}
```

### Steps
- Enforce size/type limits before upload; show errors via toast and inline text.
- Provide progress updates from API responses or XHR events.
- Support multiple files if needed; show list of selected files.

### Why this matters

- Removes Ant-specific upload logic; keeps UX clear with progress and errors.

### Gotchas

- Ensure drag/drop prevents default to avoid navigation.
- For signed URLs or multipart uploads, surface retries and failures explicitly. 
