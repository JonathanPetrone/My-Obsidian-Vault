**NOTES:** 
- I have installed a binary manually rather than via brew to avoid the Node eco-system.
- I have a way to download the latest via a script Claude helped me build, with update-tailwind in the terminal I can download the latest version.

### New Project Setup:
1. Create `input.css`:
    ```css
    @import "tailwindcss";
    ```

2. Link in HTML:
    ```html
    <link rel="stylesheet" href="output.css">
    ```

#### Commands:
- **Development:** `tailwindcss -i input.css -o output.css --watch` (auto-rebuild on changes)
- **Production:** `tailwindcss -i input.css -o output.css --minify` (one-time build)

#### Updates:
- Run `update-tailwind` to get latest version

#### Optional:
- Add `tailwind.config.js` for customization (fonts, colors, etc.)