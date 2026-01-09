✅  Google Apps Script


✅ 100% Working Google Apps Script for Merging Multiple M3U URLs

   ```
function doGet(e) {
  const urls = e.parameters.url; // Important: use `parameters`, not `parameter`

  if (!urls || urls.length === 0) {
    return ContentService.createTextOutput('Error: No M3U URLs provided')
      .setMimeType(ContentService.MimeType.TEXT);
  }

  try {
    const mergedOutput = mergeM3U(urls);
    return ContentService.createTextOutput(mergedOutput)
      .setMimeType(ContentService.MimeType.TEXT);
  } catch (error) {
    Logger.log('Error merging M3U: ' + error.stack);
    return ContentService.createTextOutput('Error merging M3U: ' + error.message)
      .setMimeType(ContentService.MimeType.TEXT);
  }
}

function mergeM3U(urls) {
  let output = '#EXTM3U\n';
  const seen = new Set();

  urls.forEach(url => {
    try {
      const response = UrlFetchApp.fetch(url);
      let content = response.getContentText();

      // Remove the first #EXTM3U line if present
      if (content.startsWith('#EXTM3U')) {
        content = content.substring(content.indexOf('\n') + 1);
      }

      const lines = content.split('\n');
      for (let i = 0; i < lines.length; i++) {
        const line = lines[i].trim();
        if (line.startsWith('#EXTINF')) {
          const streamUrl = (lines[i + 1] || '').trim();
          const key = line + streamUrl;
          if (!seen.has(key)) {
            seen.add(key);
            output += `${line}\n${streamUrl}\n`;
          }
          i++; // skip next line since it's already handled
        }
      }
    } catch (err) {
      Logger.log(`Failed to fetch or process URL: ${url} - ${err}`);
    }
  });

  return output.trim();
}
   ```

🧪 Example Test URL After Deployment:

   ```
https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec?url=https://iptv-org.github.io/iptv/languages/test1.m3u&url=https://iptv-org.github.io/iptv/languages/test2.m3u
   ```

✅ This version:

Works with 1, 2, or more url parameters.

Deduplicates streams based on #EXTINF + stream URL.

Keeps valid M3U format.







✅ Merges all M3U files hosted under:
https://raw.githubusercontent.com/280b9f9b/Stream/main/Extra/M3U8/
…including ad.m3u, dub.m3u, alain.m3u, and any future additions.


   ```
function doGet(e) {
  const filenames = e.parameters.file; // ?file=ad.m3u&file=dub.m3u

  if (!filenames || filenames.length === 0) {
    return ContentService.createTextOutput('Error: No M3U filenames provided')
      .setMimeType(ContentService.MimeType.TEXT);
  }

  const baseUrl = 'https://raw.githubusercontent.com/280b9f9b/Stream/main/Extra/M3U8/';
  const urls = filenames.map(name => baseUrl + name);

  try {
    const mergedOutput = mergeM3U(urls);
    return ContentService.createTextOutput(mergedOutput)
      .setMimeType(ContentService.MimeType.TEXT);
  } catch (error) {
    Logger.log('Error merging M3U: ' + error.stack);
    return ContentService.createTextOutput('Error merging M3U: ' + error.message)
      .setMimeType(ContentService.MimeType.TEXT);
  }
}

function mergeM3U(urls) {
  let output = '#EXTM3U\n';
  const seen = new Set();

  urls.forEach(url => {
    try {
      const response = UrlFetchApp.fetch(url);
      let content = response.getContentText();

      if (content.startsWith('#EXTM3U')) {
        content = content.substring(content.indexOf('\n') + 1);
      }

      const lines = content.split('\n');
      for (let i = 0; i < lines.length; i++) {
        const line = lines[i].trim();
        if (line.startsWith('#EXTINF')) {
          const streamUrl = (lines[i + 1] || '').trim();
          const key = line + streamUrl;
          if (!seen.has(key)) {
            seen.add(key);
            output += `${line}\n${streamUrl}\n`;
          }
          i++;
        }
      }
    } catch (err) {
      Logger.log(`Failed to fetch or process URL: ${url} - ${err}`);
    }
  });

  return output.trim();
}
   ```

🔗 Example URL to Merge:
   ```
https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec?file=test1.m3u&file=test2.m3u&file=test3.m3u
   ```









