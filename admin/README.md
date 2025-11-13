# Battle Bros Admin - Content Editor

A web-based admin interface for managing the Battle Bros comic reader website without touching code.

## 🎮 Features

### Core Functionality
- ✅ **Simple Authentication** - Password-protected access to admin panel
- ✅ **Chapter Management** - Add, edit, delete, and reorder chapters
- ✅ **Visual Editor** - Edit chapter names and page lists with intuitive UI
- ✅ **Preview & Export** - Preview changes and export JSON data
- ✅ **Draft Auto-Save** - Changes saved to localStorage automatically
- ✅ **Mobile Responsive** - Works on desktop, tablet, and mobile devices

### Design
- 🎨 **Retro Arcade Aesthetic** - Matches Battle Bros neon cyberpunk theme
- ⚡ **Smooth Animations** - Scanline effects, neon glows, and transitions
- 🎯 **Intuitive UX** - Clear visual hierarchy and minimal learning curve

## 🚀 Getting Started

### Accessing the Admin Panel

1. Navigate to `/admin/` in your web browser
2. Enter the admin password: `battlebros2024`
3. Start editing!

### Default Password

⚠️ **IMPORTANT**: Change the default password for production use!

Current password: `battlebros2024`

To change the password, edit line 684 in `/admin/index.html`:
```javascript
const ADMIN_PASSWORD = 'your-new-password-here';
```

## 📖 User Guide

### Managing Chapters

#### View All Chapters
The main dashboard shows all chapters with:
- Chapter name
- Number of pages
- Edit and Delete buttons

#### Edit a Chapter
1. Click the **EDIT** button on any chapter
2. Modify the chapter name
3. Add or remove page paths
4. Click **SAVE CHANGES**

#### Add a New Chapter
1. Click the **+ ADD NEW CHAPTER** button
2. Enter a chapter name (e.g., "Chapter 8")
3. Add page paths (e.g., "chapters/08/01.png")
4. Click **SAVE CHANGES**

#### Delete a Chapter
1. Click the **DELETE** button on any chapter
2. Confirm the deletion
3. Chapter is removed (can be recovered from localStorage if needed)

### Adding/Removing Pages

When editing a chapter:
- Click **+ ADD PAGE** to add a new image path
- Enter the full path (e.g., `chapters/01/01.png`)
- Click **Remove** next to any page to delete it
- Pages are displayed in order

### Preview & Export

1. Click **PREVIEW CHANGES** to see the JSON output
2. Review the chapter data structure
3. Click **COPY TO CLIPBOARD** to copy the JSON
4. Or click **DOWNLOAD JSON** to save as a file

### Publishing Changes

After editing:
1. Click **PREVIEW CHANGES**
2. Copy the JSON data
3. Open `index.html` in your code editor
4. Find the `chapters` object (around line 1762)
5. Replace it with your new JSON
6. Save and deploy the updated `index.html`

## 🔒 Security Features

### Current Implementation (Prototype)
- Session-based authentication using sessionStorage
- Password validation on login
- XSS protection via HTML escaping
- No external dependencies

### Production Recommendations
⚠️ This is a **PROTOTYPE** - For production use, implement:

1. **Stronger Authentication**
   - Use a backend authentication system
   - Implement JWT or session tokens
   - Add rate limiting for login attempts
   - Use environment variables for passwords

2. **HTTPS Only**
   - Always serve admin panel over HTTPS
   - Use secure cookies/storage

3. **Input Validation**
   - Server-side validation of all inputs
   - File path validation to prevent directory traversal
   - Sanitize all user inputs

4. **Access Control**
   - Multi-user support with role-based access
   - Audit logging of all changes
   - IP whitelisting if needed

## 💾 Data Storage

### How It Works

The admin interface uses **localStorage** for draft storage:
- Changes are auto-saved to browser localStorage
- Data persists across browser sessions
- No server-side database required for prototype

### Storage Keys
- `battlebros_admin_data` - Chapter data (JSON)
- `battlebros_admin_session` - Authentication session

### Clearing Data
To reset to defaults, open browser console and run:
```javascript
localStorage.removeItem('battlebros_admin_data');
```

## 🎨 Design System

### Color Palette
```css
--primary: #00d9ff    /* Cyan - main accent */
--secondary: #ff00ea  /* Magenta - secondary accent */
--accent: #ffed00     /* Yellow - highlights */
--bg-dark: #0a0a12    /* Dark background */
--bg-panel: #1a1a2e   /* Panel background */
--danger: #ff3838     /* Error/delete actions */
--success: #00ff88    /* Success states */
```

### Typography
- **Headings**: Bebas Neue (bold, condensed)
- **Body**: Righteous (geometric, retro)

### Effects
- Scanline animation (retro CRT aesthetic)
- Neon glow on interactive elements
- Glitch text effect on titles
- Smooth transitions and hover states

## 📱 Mobile Support

The admin panel is fully responsive:
- **Desktop**: Full-width layout with side-by-side controls
- **Tablet**: Adjusted padding and stacked controls
- **Mobile**: Single-column layout, touch-friendly buttons

Tested on:
- Desktop browsers (Chrome, Firefox, Safari, Edge)
- Tablet (iPad, Android tablets)
- Mobile (iPhone, Android phones)

## 🛠️ Technical Details

### Tech Stack
- Pure HTML/CSS/JavaScript (no frameworks)
- No build process required
- No external dependencies (except Google Fonts)
- Single-file application (~29KB)

### Browser Support
- Chrome/Edge (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

### File Structure
```
admin/
├── index.html          # Main admin application (single file)
└── README.md           # This file
```

## 🔧 Customization

### Changing the Look
Edit the CSS variables in the `<style>` section (lines 14-22):
```css
:root {
  --primary: #00d9ff;    /* Change colors here */
  --secondary: #ff00ea;
  --accent: #ffed00;
  /* ... */
}
```

### Modifying Features
The JavaScript code is organized into sections:
- **Authentication** (lines 655-690)
- **Data Management** (lines 693-821)
- **UI Rendering** (lines 824-859)
- **Chapter Operations** (lines 862-951)
- **Preview & Export** (lines 967-1013)

## 🐛 Troubleshooting

### Login Issues
**Problem**: Can't log in with the password
- **Solution**: Check that password is exactly `battlebros2024` (case-sensitive)
- Clear browser cache and try again

### Changes Not Saving
**Problem**: Edits disappear after refresh
- **Solution**: Check browser console for errors
- Ensure localStorage is enabled in browser
- Try a different browser

### JSON Export Issues
**Problem**: Can't copy or download JSON
- **Solution**: Grant clipboard permissions when prompted
- Try the Download button instead of Copy
- Check browser console for errors

### Mobile Issues
**Problem**: Interface not responsive on mobile
- **Solution**: Ensure viewport meta tag is present
- Clear browser cache
- Try landscape orientation for better view

## 📞 Support

For issues or questions:
1. Check this README first
2. Review the `ADMIN_GUI_EDITOR_SPEC.md` in `/new_feature/`
3. Check browser console for error messages
4. Consult the main repository documentation

## 🚧 Future Enhancements

Planned features (not in current prototype):
- Image upload interface
- Bulk chapter operations
- Email subscriber management
- Analytics dashboard
- Scheduled publishing
- Multi-user support with roles
- Version history and rollback
- Real-time preview of changes
- Drag-and-drop page reordering
- Image optimization tools

## 📝 License

Part of the Battle Bros Comic Reader project.
See main repository for license information.

---

**Remember**: This is a prototype for demonstration purposes. Implement proper security measures before using in production!
