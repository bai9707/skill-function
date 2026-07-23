---
name: image-compress
description: Automated image compression workflow using server-side smart compression. Supports local paths, folders, and URLs with CDN output and compression ratio reporting. Use when compressing images, reducing file size, or optimizing photos.
license: MIT
compatibility: Requires nodejs and nx-mcp-server with NX_API_KEY configured
metadata:
  author: xiaowu89
  version: 1.0.0
  tags: image-compress, compression, optimization, media
---

# Image Compression

Compress images using the nx-mcp-server remote compression service.

## First-Time Setup

The skill auto-detects MCP configuration on first run. If the `nx_compress` tool is unavailable:

### 1. Install MCP Server

Create `.mcp.json` in your project directory:

```json
{
  "mcpServers": {
    "nx-mcp-compress": {
      "type": "url",
      "url": "https://mcp.api-inference.modelscope.net/da691d14ea0d46/mcp",
      "env": {
        "NX_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

Restart Claude Code after adding.

### 2. Configure API Key

If MCP returns `MISSING_API_KEY` or `API_AUTH_FAILED`:
- Check that `env.NX_API_KEY` is set in `.mcp.json`
- **No API Key?** Contact WeChat `xiaowu89` to get one
- Restart Claude Code after updating

## Usage

Say "compress these images" and the skill will:

1. Collect images from a folder path or URL
2. Convert local files to dataUrl (remote MCP cannot access local disk)
3. Call the nx_compress MCP tool with dataUrl or HTTP URLs
4. Summarize compression results in a table with size comparison and ratio

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `urls` | `string[]` | Yes* | — | HTTP URLs or local paths |
| `files` | `string[]` | Yes* | — | dataUrl format images |
| `quality` | `integer` | No | `90` | 1-100, higher is better |
| `output` | `string` | No | — | `"overwrite"` or directory path |

## Response Fields

- `originalSize` — original file size in bytes
- `compressedSize` — compressed file size in bytes
- `ratio` — compression ratio (e.g. "87.4%")
- `compressedUrl` — CDN URL of compressed image
- `summary` — aggregate stats (total, success, failed)

## Size Limit

Remote MCP limits single file to **5MB**. Files exceeding this are rejected.

## Supported Formats

png, jpg, jpeg, bmp, webp, tga
