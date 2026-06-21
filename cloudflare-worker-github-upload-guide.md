# Anonymous File Uploads to GitHub Pages Using Cloudflare Workers

## Architecture

```text
Anonymous User
       ↓
HTML Upload Form
       ↓
Cloudflare Worker
       ↓
GitHub API
       ↓
GitHub Repository
       ↓
GitHub Action (optional)
       ↓
files.json
       ↓
Random Redirect Page
```

## Step 1: Create a GitHub Token

Settings → Developer Settings → Personal Access Tokens → Fine-grained Token

Permission:

```text
Contents: Read and Write
```

## Step 2: Store the Token in Cloudflare

Worker → Settings → Variables

Secret:

```text
GITHUB_TOKEN
```

## Step 3: Upload Form

```html
<input type="file" id="file">
<button onclick="upload()">Upload</button>

<script>
async function upload() {
  const file = document.getElementById("file").files[0];

  const formData = new FormData();
  formData.append("file", file);

  await fetch(
    "https://your-worker.workers.dev/upload",
    {
      method: "POST",
      body: formData
    }
  );

  alert("Uploaded");
}
</script>
```

## Step 4: Cloudflare Worker

```javascript
export default {
  async fetch(request, env) {

    if (request.method === "POST") {

      const form = await request.formData();
      const file = form.get("file");

      const bytes = await file.arrayBuffer();

      const base64 = btoa(
        String.fromCharCode(
          ...new Uint8Array(bytes)
        )
      );

      const filename =
        Date.now() + "-" + file.name;

      return fetch(
        `https://api.github.com/repos/YOURNAME/REPO/contents/files/${filename}`,
        {
          method: "PUT",
          headers: {
            Authorization:
              `Bearer ${env.GITHUB_TOKEN}`,
            "Content-Type":
              "application/json"
          },
          body: JSON.stringify({
            message: `upload ${filename}`,
            content: base64
          })
        }
      );
    }

    return new Response("upload endpoint");
  }
}
```

## Step 5: Auto-Generate files.json

Trigger:

```yaml
on:
  push:
```

Flow:

```text
New File
    ↓
GitHub Action
    ↓
files.json
    ↓
GitHub Pages
```

## Step 6: Random Redirect

```html
<script>
fetch("files.json")
.then(r => r.json())
.then(files => {

  const file =
    files[
      Math.floor(
        Math.random() *
        files.length
      )
    ];

  location.href = file;
});
</script>
```

## Alternative: Skip files.json

Worker queries:

```text
GET /repos/USER/REPO/contents/files
```

Benefits:

- Anonymous uploads
- Automatic commits
- Automatic file discovery
- Automatic redirects
- No manual updates
