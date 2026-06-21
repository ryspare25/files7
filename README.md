# files7

Automated random file redirector with GitHub Actions that automatically generates and updates `files.json` whenever you push changes.

## 🚀 Features

- **Random File Selection**: Automatically picks and redirects to a random file from your collection
- **Automatic Updates**: Push files to the `files/` directory and `files.json` is automatically regenerated
- **Zero Maintenance**: No manual HTML or JSON editing required
- **GitHub Actions**: Fully automated workflow on every push to main branch
- **GitHub Pages Ready**: Deployed at https://ryspare25.github.io/files7/

## 📁 Project Structure

```
repo/
├── files/                    # Place your files here
│   ├── foo.pdf
│   ├── bar.pdf
│   └── baz.pdf
├── files.json               # Auto-generated file list
├── index.html               # File browser interface
├── README.md
└── .github/
    └── workflows/
        └── generate-files-json.yml  # GitHub Action workflow
```

## 🔧 How It Works

1. **Add Files**: Drag and drop files into the `/files` directory
2. **Commit & Push**: Push your changes to GitHub
3. **Automatic Generation**: GitHub Action scans the `files/` directory
4. **Update files.json**: The workflow automatically updates `files.json`
5. **Random Redirect**: Visit `index.html` and get redirected to a random file

## 📝 Workflow

The GitHub Action (`.github/workflows/generate-files-json.yml`) runs on every push to the `main` branch:

```yaml
- Checks out the repository
- Scans the files/ directory
- Generates files.json with all filenames
- Commits and pushes the updated files.json
```

## 🎯 Usage

### Adding New Files

1. Add your files to the `files/` directory:
   ```bash
   cp /path/to/your/file.pdf files/
   ```

2. Commit and push:
   ```bash
   git add files/
   git commit -m "Add new file"
   git push origin main
   ```

3. The GitHub Action will automatically:
   - Detect the new file
   - Update `files.json`
   - Commit the changes
   - Your `index.html` will show the new file immediately

### Using the Random File Redirector

Visit the page to get redirected to a random file:

- **GitHub Pages**: https://ryspare25.github.io/files7/
- **Local**: Open `index.html` directly in your browser

Each visit picks a different random file from your collection!

## 🛠️ Technical Details

### GitHub Action Workflow

The workflow uses:
- `find` to locate all files in the `files/` directory
- `sed` to clean up file paths
- `jq` to format the output as JSON
- Git commands to commit and push changes automatically

### Random File Redirector (index.html)

Features:
- Fetches `files.json` dynamically
- Picks a random file from the list
- Automatically redirects to the selected file
- Shows loading animation during selection
- Error handling for missing files
- Works seamlessly with GitHub Pages

## 🔒 Permissions

The GitHub Action requires write permissions to commit changes. This is automatically granted to `github-actions` bot.

## 📄 Supported File Types

The system works with any file type:
- PDFs
- Images (PNG, JPG, GIF, etc.)
- Documents (DOCX, TXT, etc.)
- Archives (ZIP, TAR, etc.)
- Any other file format

## 🎨 Customization

### Modify the Random Redirector

Edit `index.html` to customize:
- Styling and colors
- Loading animation
- Redirect delay timing
- Add file selection logic (e.g., weighted random, sequential)

### Change Workflow Trigger

Edit `.github/workflows/generate-files-json.yml` to trigger on:
- Different branches
- Pull requests
- Manual workflow dispatch
- Scheduled runs

## 📊 Example files.json

```json
[
  "bar.pdf",
  "baz.pdf",
  "foo.pdf"
]
```

## 🚦 Getting Started

1. Clone this repository
2. Add your files to the `files/` directory
3. Push to GitHub
4. Watch the magic happen! ✨

## 📜 License

This project is open source and available for any use.

## 🤝 Contributing

Feel free to submit issues or pull requests to improve this project!

---

**Note**: The first time you push, make sure the GitHub Action has completed before viewing `index.html` to ensure `files.json` is properly generated.