# Image Upload Implementation Guide for Battle Bros Admin Panel

## Overview
This document provides complete instructions for implementing image upload functionality in the Battle Bros admin panel. The feature will allow uploading images directly through the browser interface and committing them to the GitHub repository using the GitHub API.

## Current State Analysis

### What Already Exists
- Admin panel at `/admin/index.html` in `feature/admin-panel` branch
- GitHub API integration framework (lines 776-781)
- GitHub token storage in localStorage (`GITHUB_TOKEN_KEY`)
- Chapter and page management UI
- Manual path entry for images (line 726 - "Add Page" button)

### What's Missing
- File input/upload interface
- Image file handling (reading, validation, base64 conversion)
- GitHub API file upload implementation
- Progress indicators during upload
- Image preview thumbnails
- Automatic file naming and path generation

## Implementation Plan

### Phase 1: Add File Upload UI

#### Step 1.1: Add File Input to Edit Modal
**Location:** `admin/index.html` around line 726

**Current Code:**
```html
<div class="form-group">
  <label class="form-label">Pages (Image Paths)</label>
  <div id="pageList" class="page-list"></div>
  <button type="button" class="btn-small btn-add-page" id="btnAddPage">+ Add Page</button>
</div>
```

**Replace With:**
```html
<div class="form-group">
  <label class="form-label">Pages (Image Paths)</label>
  <div id="pageList" class="page-list"></div>
  
  <!-- Upload Section -->
  <div class="upload-section" style="margin-top: 15px;">
    <label class="form-label">Upload New Images</label>
    <div class="upload-area" id="uploadArea">
      <input 
        type="file" 
        id="imageUpload" 
        accept="image/png,image/jpeg,image/jpg,image/gif,image/webp"
        multiple
        style="display: none;"
      />
      <div class="upload-prompt" id="uploadPrompt">
        <p>📁 Click to browse or drag & drop images here</p>
        <p style="font-size: 0.8rem; opacity: 0.7;">Supports: PNG, JPG, GIF, WebP</p>
      </div>
      <div id="uploadPreview" class="upload-preview"></div>
    </div>
    <button type="button" class="btn-small btn-add-page" id="btnUploadImages" style="display: none;">
      ⬆️ Upload Selected Images
    </button>
    <div id="uploadProgress" class="upload-progress" style="display: none;"></div>
  </div>
  
  <!-- Manual Path Entry (keep existing) -->
  <div style="margin-top: 15px;">
    <button type="button" class="btn-small btn-add-page" id="btnAddPage">+ Add Manual Path</button>
  </div>
</div>
```

#### Step 1.2: Add CSS Styles for Upload UI
**Location:** Add to `<style>` section around line 614

```css
/* ==================== IMAGE UPLOAD STYLES ==================== */
.upload-section {
  background: var(--bg-dark);
  border: 2px solid var(--accent);
  border-radius: 5px;
  padding: 15px;
}

.upload-area {
  border: 2px dashed var(--primary);
  border-radius: 5px;
  padding: 20px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  background: var(--bg-panel);
  min-height: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.upload-area:hover {
  border-color: var(--accent);
  background: rgba(255, 237, 0, 0.05);
}

.upload-area.drag-over {
  border-color: var(--success);
  background: rgba(0, 255, 136, 0.1);
  box-shadow: 0 0 20px rgba(0, 255, 136, 0.3);
}

.upload-prompt {
  color: var(--primary);
  pointer-events: none;
}

.upload-preview {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 10px;
  margin-top: 15px;
  width: 100%;
}

.preview-item {
  position: relative;
  border: 2px solid var(--primary);
  border-radius: 5px;
  overflow: hidden;
  aspect-ratio: 1;
  background: var(--bg-dark);
}

.preview-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.preview-item .preview-name {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(10, 10, 18, 0.9);
  color: var(--text);
  font-size: 0.7rem;
  padding: 4px;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.preview-item .preview-remove {
  position: absolute;
  top: 5px;
  right: 5px;
  background: var(--danger);
  color: var(--text);
  border: none;
  width: 24px;
  height: 24px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 0.9rem;
  line-height: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.preview-item .preview-remove:hover {
  background: var(--text);
  color: var(--danger);
}

.upload-progress {
  margin-top: 15px;
  background: var(--bg-panel);
  border: 2px solid var(--primary);
  border-radius: 5px;
  padding: 15px;
}

.progress-item {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
  font-size: 0.85rem;
}

.progress-bar-container {
  flex: 1;
  height: 20px;
  background: var(--bg-dark);
  border: 1px solid var(--primary);
  border-radius: 3px;
  overflow: hidden;
}

.progress-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--primary), var(--accent));
  transition: width 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.7rem;
  color: var(--bg-dark);
  font-weight: bold;
}

.progress-status {
  min-width: 80px;
  text-align: right;
}

.progress-status.success {
  color: var(--success);
}

.progress-status.error {
  color: var(--danger);
}
```

### Phase 2: Implement File Handling Logic

#### Step 2.1: Add File Selection Variables
**Location:** Add to state management section around line 784

```javascript
// Image upload state
let selectedFiles = [];
let uploadQueue = [];
```

#### Step 2.2: Implement File Selection Handler
**Location:** Add new function in JavaScript section around line 1000

```javascript
// ==================== IMAGE UPLOAD HANDLERS ====================

function initUploadHandlers() {
  const uploadArea = document.getElementById('uploadArea');
  const fileInput = document.getElementById('imageUpload');
  const uploadPrompt = document.getElementById('uploadPrompt');
  const uploadPreview = document.getElementById('uploadPreview');
  const btnUploadImages = document.getElementById('btnUploadImages');

  // Click to browse
  uploadArea.addEventListener('click', () => {
    fileInput.click();
  });

  // File selection
  fileInput.addEventListener('change', (e) => {
    handleFileSelect(e.target.files);
  });

  // Drag and drop
  uploadArea.addEventListener('dragover', (e) => {
    e.preventDefault();
    e.stopPropagation();
    uploadArea.classList.add('drag-over');
  });

  uploadArea.addEventListener('dragleave', (e) => {
    e.preventDefault();
    e.stopPropagation();
    uploadArea.classList.remove('drag-over');
  });

  uploadArea.addEventListener('drop', (e) => {
    e.preventDefault();
    e.stopPropagation();
    uploadArea.classList.remove('drag-over');
    handleFileSelect(e.dataTransfer.files);
  });

  // Upload button
  btnUploadImages.addEventListener('click', () => {
    uploadImagesToGitHub();
  });
}

function handleFileSelect(files) {
  const uploadPrompt = document.getElementById('uploadPrompt');
  const uploadPreview = document.getElementById('uploadPreview');
  const btnUploadImages = document.getElementById('btnUploadImages');

  // Filter for valid image files
  const imageFiles = Array.from(files).filter(file => {
    return file.type.startsWith('image/');
  });

  if (imageFiles.length === 0) {
    showError('Please select valid image files (PNG, JPG, GIF, WebP)');
    return;
  }

  // Add to selected files (avoid duplicates)
  imageFiles.forEach(file => {
    const exists = selectedFiles.find(f => f.name === file.name && f.size === file.size);
    if (!exists) {
      selectedFiles.push(file);
    }
  });

  // Update UI
  uploadPrompt.style.display = selectedFiles.length > 0 ? 'none' : 'block';
  btnUploadImages.style.display = selectedFiles.length > 0 ? 'block' : 'none';

  renderFilePreview();
}

function renderFilePreview() {
  const uploadPreview = document.getElementById('uploadPreview');
  uploadPreview.innerHTML = '';

  selectedFiles.forEach((file, index) => {
    const reader = new FileReader();
    
    reader.onload = (e) => {
      const previewItem = document.createElement('div');
      previewItem.className = 'preview-item';
      previewItem.innerHTML = `
        <img src="${e.target.result}" alt="${file.name}">
        <div class="preview-name" title="${file.name}">${file.name}</div>
        <button class="preview-remove" data-index="${index}" title="Remove">×</button>
      `;
      
      // Remove button handler
      previewItem.querySelector('.preview-remove').addEventListener('click', (evt) => {
        evt.stopPropagation();
        removeSelectedFile(index);
      });
      
      uploadPreview.appendChild(previewItem);
    };
    
    reader.readAsDataURL(file);
  });
}

function removeSelectedFile(index) {
  selectedFiles.splice(index, 1);
  
  const uploadPrompt = document.getElementById('uploadPrompt');
  const btnUploadImages = document.getElementById('btnUploadImages');
  
  uploadPrompt.style.display = selectedFiles.length > 0 ? 'none' : 'block';
  btnUploadImages.style.display = selectedFiles.length > 0 ? 'block' : 'none';
  
  renderFilePreview();
}

function clearSelectedFiles() {
  selectedFiles = [];
  const uploadPrompt = document.getElementById('uploadPrompt');
  const uploadPreview = document.getElementById('uploadPreview');
  const btnUploadImages = document.getElementById('btnUploadImages');
  const fileInput = document.getElementById('imageUpload');
  
  uploadPrompt.style.display = 'block';
  uploadPreview.innerHTML = '';
  btnUploadImages.style.display = 'none';
  fileInput.value = '';
}
```

### Phase 3: Implement GitHub API Upload

#### Step 3.1: Add GitHub File Upload Function
**Location:** Add after other GitHub functions around line 1100

```javascript
// ==================== GITHUB API - FILE UPLOAD ====================

async function uploadImagesToGitHub() {
  const token = localStorage.getItem(GITHUB_TOKEN_KEY);
  
  if (!token) {
    showError('Please configure your GitHub token in Settings first.');
    document.getElementById('btnSettings').click();
    return;
  }

  if (!currentEditingChapter) {
    showError('No chapter is currently being edited.');
    return;
  }

  if (selectedFiles.length === 0) {
    showError('No files selected for upload.');
    return;
  }

  const uploadProgress = document.getElementById('uploadProgress');
  const btnUploadImages = document.getElementById('btnUploadImages');
  
  uploadProgress.style.display = 'block';
  uploadProgress.innerHTML = '';
  btnUploadImages.disabled = true;
  btnUploadImages.textContent = 'Uploading...';

  const results = [];
  
  for (let i = 0; i < selectedFiles.length; i++) {
    const file = selectedFiles[i];
    const result = await uploadSingleImage(file, i + 1, selectedFiles.length, token);
    results.push(result);
  }

  // Check if all succeeded
  const allSuccess = results.every(r => r.success);
  
  if (allSuccess) {
    // Add all uploaded paths to current chapter
    results.forEach(r => {
      if (r.path && !chapters[currentEditingChapter].includes(r.path)) {
        chapters[currentEditingChapter].push(r.path);
      }
    });
    
    saveChapters();
    renderPageList();
    clearSelectedFiles();
    
    showSuccess(`Successfully uploaded ${results.length} image(s)!`);
    
    setTimeout(() => {
      uploadProgress.style.display = 'none';
    }, 3000);
  } else {
    const failedCount = results.filter(r => !r.success).length;
    showError(`${failedCount} of ${results.length} uploads failed. Check the progress details.`);
  }

  btnUploadImages.disabled = false;
  btnUploadImages.textContent = '⬆️ Upload Selected Images';
}

async function uploadSingleImage(file, index, total, token) {
  const progressItem = document.createElement('div');
  progressItem.className = 'progress-item';
  progressItem.innerHTML = `
    <span>${file.name}</span>
    <div class="progress-bar-container">
      <div class="progress-bar-fill" style="width: 0%">0%</div>
    </div>
    <span class="progress-status">Uploading...</span>
  `;
  
  document.getElementById('uploadProgress').appendChild(progressItem);
  const progressBar = progressItem.querySelector('.progress-bar-fill');
  const progressStatus = progressItem.querySelector('.progress-status');

  try {
    // Generate file path
    const chapterNumber = currentEditingChapter.match(/\d+/)?.[0] || '01';
    const chapterFolder = `chapters/${chapterNumber.padStart(2, '0')}`;
    
    // Get next available number in chapter
    const existingPages = chapters[currentEditingChapter] || [];
    const existingNumbers = existingPages
      .filter(p => p.startsWith(chapterFolder))
      .map(p => {
        const match = p.match(/\/(\d+)\.(png|jpg|jpeg|gif|webp)$/i);
        return match ? parseInt(match[1]) : 0;
      })
      .filter(n => !isNaN(n));
    
    const nextNumber = existingNumbers.length > 0 ? Math.max(...existingNumbers) + 1 : 1;
    const fileExt = file.name.split('.').pop().toLowerCase();
    const fileName = `${nextNumber.toString().padStart(2, '0')}.${fileExt}`;
    const filePath = `${chapterFolder}/${fileName}`;

    // Read file as base64
    progressBar.style.width = '30%';
    progressBar.textContent = '30%';
    const base64Content = await readFileAsBase64(file);

    // Upload to GitHub
    progressBar.style.width = '60%';
    progressBar.textContent = '60%';
    
    const response = await fetch(
      `https://api.github.com/repos/${GITHUB_CONFIG.owner}/${GITHUB_CONFIG.repo}/contents/${filePath}`,
      {
        method: 'PUT',
        headers: {
          'Authorization': `token ${token}`,
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          message: `Add ${fileName} to ${currentEditingChapter}`,
          content: base64Content,
          branch: GITHUB_CONFIG.branch
        })
      }
    );

    progressBar.style.width = '100%';
    progressBar.textContent = '100%';

    if (response.ok) {
      progressStatus.textContent = '✓ Success';
      progressStatus.classList.add('success');
      return { success: true, path: filePath, file: file.name };
    } else {
      const error = await response.json();
      throw new Error(error.message || 'Upload failed');
    }

  } catch (error) {
    console.error('Upload error:', error);
    progressBar.style.width = '100%';
    progressBar.textContent = 'Failed';
    progressBar.style.background = 'var(--danger)';
    progressStatus.textContent = `✗ ${error.message}`;
    progressStatus.classList.add('error');
    return { success: false, error: error.message, file: file.name };
  }
}

function readFileAsBase64(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => {
      // Remove data URL prefix (data:image/png;base64,)
      const base64 = reader.result.split(',')[1];
      resolve(base64);
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

function showError(message) {
  // Reuse existing error display or create alert
  alert('ERROR: ' + message);
}

function showSuccess(message) {
  // Reuse existing success display or create alert
  alert('SUCCESS: ' + message);
}
```

#### Step 3.2: Initialize Upload Handlers
**Location:** Modify the `showDashboard()` function around line 845

```javascript
function showDashboard() {
  el.loginScreen.style.display = 'none';
  el.adminDashboard.style.display = 'block';
  loadChapters();
  renderChapterList();
  initUploadHandlers(); // ADD THIS LINE
}
```

### Phase 4: Handle Edge Cases

#### Step 4.1: Clear Upload State When Closing Modal
**Location:** Modify modal close handlers around line 1050

```javascript
function closeEditModal() {
  el.editModal.classList.remove('active');
  currentEditingChapter = null;
  clearSelectedFiles(); // ADD THIS LINE
}

// Also add to btnCloseModal and btnCancelEdit click handlers
el.btnCloseModal.addEventListener('click', () => {
  closeEditModal();
});

el.btnCancelEdit.addEventListener('click', () => {
  closeEditModal();
});
```

#### Step 4.2: Add File Size Validation
**Location:** Modify `handleFileSelect()` function

```javascript
function handleFileSelect(files) {
  const MAX_FILE_SIZE = 10 * 1024 * 1024; // 10MB limit
  
  const uploadPrompt = document.getElementById('uploadPrompt');
  const uploadPreview = document.getElementById('uploadPreview');
  const btnUploadImages = document.getElementById('btnUploadImages');

  // Filter for valid image files
  const imageFiles = Array.from(files).filter(file => {
    if (!file.type.startsWith('image/')) {
      return false;
    }
    if (file.size > MAX_FILE_SIZE) {
      showError(`File ${file.name} is too large (max 10MB)`);
      return false;
    }
    return true;
  });

  if (imageFiles.length === 0) {
    showError('Please select valid image files (PNG, JPG, GIF, WebP) under 10MB');
    return;
  }

  // ... rest of function
}
```

## Testing Checklist

### Local Testing
- [ ] File input opens when clicking upload area
- [ ] Drag and drop works
- [ ] Image previews display correctly
- [ ] Remove button works for each preview
- [ ] Multiple files can be selected
- [ ] File size validation works
- [ ] Invalid file types are rejected

### GitHub Integration Testing
- [ ] GitHub token can be saved
- [ ] Upload progress displays for each file
- [ ] Files are committed to correct chapter folder
- [ ] Auto-numbering works correctly (01.png, 02.png, etc.)
- [ ] Paths are automatically added to chapter
- [ ] Error handling works for failed uploads
- [ ] Success message displays after upload

### UI/UX Testing
- [ ] Upload area highlights on drag-over
- [ ] Progress bars update smoothly
- [ ] Success/error states are clear
- [ ] Modal can be closed during/after upload
- [ ] State clears properly when modal closes

## Security Considerations

1. **Token Storage**: GitHub token is stored in localStorage (client-side only)
2. **File Validation**: Only allow image types, enforce size limits
3. **Path Sanitization**: Ensure generated file paths are safe
4. **Rate Limiting**: GitHub API has rate limits - consider batch uploads
5. **Authentication**: Admin password should be environment variable in production

## Deployment Steps

1. **Test Locally**: Open `admin/index.html` in browser
2. **Set GitHub Token**: Configure in Settings modal
3. **Test Upload**: Upload a test image to a chapter
4. **Verify GitHub**: Check that file appears in repository
5. **Merge Branch**: Merge `feature/admin-panel` to main when ready

## API Rate Limits

GitHub API (authenticated):
- 5,000 requests per hour
- File size limit: 100MB per file
- Recommended: Upload max 50 images per session

## Future Enhancements

1. **Image Optimization**: Compress images before upload
2. **Batch Processing**: Upload multiple chapters at once
3. **WebP Conversion**: Auto-convert to WebP for better performance
4. **Thumbnail Generation**: Create optimized thumbnails
5. **Duplicate Detection**: Check for existing images before upload
6. **Cloud Storage Integration**: Use Cloudflare Images or AWS S3

## Troubleshooting

### "Upload failed" error
- Check GitHub token has `repo` scope
- Verify branch name in `GITHUB_CONFIG`
- Ensure file path doesn't already exist

### Images not appearing in chapter
- Check `saveChapters()` is called after upload
- Verify paths are added to `chapters` object
- Check localStorage for saved data

### Drag and drop not working
- Ensure `preventDefault()` is called on drag events
- Check browser console for errors
- Test in different browsers

## Code Location Reference

- **Main admin file**: `/admin/index.html` in `feature/admin-panel` branch
- **GitHub config**: Lines 776-781
- **Chapter data**: Lines 867-999
- **Modal HTML**: Lines 704-735
- **JavaScript**: Lines 765+

## Support

For issues or questions about implementation:
1. Check browser console for errors
2. Verify GitHub API response in Network tab
3. Test with small images first
4. Review GitHub API documentation: https://docs.github.com/en/rest/repos/contents

---

**Implementation Time Estimate**: 2-3 hours for complete feature
**Difficulty**: Intermediate (requires JavaScript async/await knowledge)
**Dependencies**: GitHub Personal Access Token with `repo` scope