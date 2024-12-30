I am leaving the javascript inside of the html... But it could be moved outside and here are the instructions from chatGPT.


Yes, the script you provided at the beginning of your message is **already included and referenced in the HTML document you shared**. It is embedded directly within the `<script>` tag inside the `<body>` of your HTML. Here’s how it looks in your provided HTML:

```html
<script>
    document.addEventListener('DOMContentLoaded', function() {
        // Get query parameters
        const urlParams = new URLSearchParams(window.location.search);
        const imageUrl = urlParams.get('imageURL');

        // Set the image source or display a loading GIF
        const sharedImage = document.getElementById('shared-image');
        if (sharedImage) {
            if (imageUrl) {
                sharedImage.src = imageUrl;
            }
        }

        // Copy discount link functionality
        const copyLinkButton = document.getElementById('copyLinkButton');
        if (copyLinkButton) {
            copyLinkButton.addEventListener('click', function() {
                navigator.clipboard.writeText('JustAddLuv.com/discount/LUV15').then(() => {
                    alert('Discount link "JustAddLuv.com/discount/LUV15" copied to clipboard! Paste it as a link in your stories and in your posts or comments so your friends and family can more easily get their 15% discount! Thank You :)');
                }).catch(err => {
                    console.error('Failed to copy text:', err);
                });
            });
        }

        // Share image functionality
        const shareButton = document.getElementById('shareButton');
        if (shareButton) {
            shareButton.addEventListener('click', async function() {
                if (imageUrl && navigator.share) {
                    try {
                        const response = await fetch(imageUrl);
                        const blob = await response.blob();

                        const fileType = imageUrl.endsWith('.png') ? 'image/png' : 'image/jpeg';
                        const fileExtension = fileType.split('/')[1];

                        const file = new File([blob], `image.${fileExtension}`, { type: fileType });

                        await navigator.share({
                            files: [file], 
                        });

                        setTimeout(function() {
                            // Redirect to the JustAddLuv rewards page with all query parameters
                            window.location.href = `https://reward.embrfyr.com/JustAddLuv?${urlParams.toString()}`;
                        }, 1000);
                    } catch (error) {
                        console.log('Sharing failed', error);
                    }
                } else {
                    console.log('Image URL not found or Web Share API not supported.');
                }
            });
        }

        // Append Hyperise script to the document
        const hyperiseScript = document.createElement('script');
        hyperiseScript.src = 'https://app.hyperise.io/snippets/r/v2/r31G4FYxU.js?v=1.0.0';
        hyperiseScript.async = true; // Load the script asynchronously
        hyperiseScript.onload = function() {
            if (typeof hyperise !== 'undefined') {
                hyperise.init();
            }
        };
        document.body.appendChild(hyperiseScript);
    });
</script>
```

### Key Points:
- **Embedded Script:** The script is written directly within the `<script>` tag and is already part of the `index.html` file.
- **Functionality:** It performs tasks like:
  - Parsing query parameters (`imageURL`) to update the `src` attribute of an image.
  - Handling button events for sharing and copying discount links.
  - Dynamically appending an external script (Hyperise).
- **Placement:** The `<script>` tag is located just before the closing `</body>` tag, which is a standard practice for scripts that interact with the DOM.

---

### **If You Want to Externalize This Script**

To make the script modular and easier to manage, you can move it into an external JavaScript file (`scripts.js`) and reference it in your HTML. Here's how:

1. **Create a `scripts.js` File**
   - Move the entire script from the `<script>` tag into a new file named `scripts.js`.

2. **Update `index.html`**
   - Remove the `<script>` block and add this line just before the closing `</body>` tag:
     ```html
     <script src="scripts.js" defer></script>
     ```

3. **Final `index.html` Example**
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="viewport" content="width=device-width, initial-scale=1.0">
       <title>Share Your Review Image</title>
       <link rel="stylesheet" href="styles.css">
   </head>
   <body>
       <div class="container">
           <div class="header">
               <img src="https://s3.typebot.io/public/workspaces/cl6blb2a6285809jpzzel4b7q/typebots/clbghkpki000wjs082fs98844/results/veqnu5xdykgotk885364anqv/JustAddLuvLogo_1220x1220.jpg" alt="JustAddLuv Logo" class="logo">
               <h1>Your image is ready to share!</h1>
               <p>Once you share, we will send you a 20% off code and link at JustAddLuv.com!</p>
           </div>

           <div class="image-container">
               <img id="shared-image" src="https://s3.typebot.io/public/workspaces/cl6blb2a6285809jpzzel4b7q/typebots/clbghkpki000wjs082fs98844/results/hk54w4ejxncepntulfyv93m1/ImageLoading_1080x1080.gif" alt="Image Loading" />
           </div>

           <div class="content">
               <button id="shareButton">Share My Review Image</button>
               <p><strong>Wait! Before you share...</strong></p>
               <p>If you want to make it easier for your friends to get the discount, you can use the button below to FIRST copy the discount link, then put that link in your post and stories. (Just tap 'Share My Review Image' if this is confusing to you—no need to worry about the link!)</p>
               <button id="copyLinkButton">Copy Discount Link</button>
           </div>
       </div>

       <!-- Reference external script -->
       <script src="scripts.js" defer></script>
   </body>
   </html>
   ```

### Advantages of Externalizing the Script:
- **Modularity:** Easier to maintain and debug.
- **Reusability:** You can use the same script across multiple pages if needed.
- **Readability:** Keeps your HTML cleaner and more focused on structure/content.

Let me know if you'd like additional help with this process!
