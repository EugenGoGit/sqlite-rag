# SQLite RAG Integration with Continue

This guide explains how to integrate SQLite RAG with the VS Code Continue extension using MCP (Model Context Protocol).

## Prerequisites

- VS Code with Continue extension installed
- SQLite RAG installed with MCP support: `pip install sqlite-rag[mcp]`
- A SQLite RAG database with indexed documents

## Quick Setup

### 1. Locate Your Continue Configuration

The Continue configuration file is typically located at:
- Windows: `%USERPROFILE%\.continue\config.json`
- macOS/Linux: `~/.continue/config.json`

### 2. Add MCP Server Configuration

Add the following to your `config.json`:

```json
{
  "mcpServers": {
    "sqlite-rag": {
      "command": "python",
      "args": [
        "-m", "sqlite_rag.mcp_server",
        "./sqliterag.sqlite"
      ],
      "env": {}
    }
  }
}
```

**Important**: Replace `./sqliterag.sqlite` with the absolute path to your SQLite RAG database file.

### 3. Restart VS Code

Restart VS Code to load the MCP server.

## Available Tools

Once configured, the following tools are available in Continue:

### `search`
Search your documents using hybrid vector + full-text search.

**Parameters:**
- `query` (string, required): The search query
- `limit` (number, optional): Number of results to return (default: 5)

**Example:**
```
Search for documents about machine learning with limit 10
```

### `add_text`
Add text content to the RAG database.

**Parameters:**
- `text` (string, required): The text content to add
- `uri` (string, optional): URI/path identifier for the text
- `metadata` (string, optional): Metadata in JSON format

**Example:**
```
Add text "SQLite is a C-language library" with URI "docs/sqlite_intro.txt"
```

### `list_documents`
List all documents in the database.

**No parameters required.**

### `remove_document`
Remove a document by ID or URI.

**Parameters:**
- `identifier` (string, required): Document ID or URI to remove

**Example:**
```
Remove document with URI "old_document.txt"
```

### `get_settings`
Get current configuration settings of the RAG system.

**No parameters required.**

## Troubleshooting

### MCP Server Not Loading

1. Check if the Python path is correct
2. Verify that the database file exists
3. Check VS Code output panel for MCP-related errors

### Search Returns No Results

1. Ensure documents are added to the database
2. Verify the database path is correct in the configuration
3. Check if the embedding model is properly loaded

### Permission Errors

1. Ensure the Python environment has read/write access to the database file
2. Check file permissions on the database file

## Advanced Configuration

### Using a Virtual Environment

If you're using a virtual environment, specify the full path to Python:

```json
{
  "mcpServers": {
    "sqlite-rag": {
      "command": "C:/path/to/.venv/Scripts/python.exe",
      "args": [
        "-m", "sqlite_rag.mcp_server",
        "C:/path/to/sqliterag.sqlite"
      ],
      "env": {}
    }
  }
}
```

### Environment Variables

You can pass environment variables to the MCP server:

```json
{
  "mcpServers": {
    "sqlite-rag": {
      "command": "python",
      "args": [
        "-m", "sqlite_rag.mcp_server",
        "./sqliterag.sqlite"
      ],
      "env": {
        "SQLITE_RAG_LOG_LEVEL": "DEBUG",
        "CUSTOM_SETTING": "value"
      }
    }
  }
}
```

## Development

To run the MCP server directly for debugging:

```bash
python -m sqlite_rag.mcp_server ./sqliterag.sqlite
```

Or install in development mode:

```bash
pip install -e '.[dev,mcp]'
python -m sqlite_rag.mcp_server ./sqliterag.sqlite
```

## Support

For issues or questions, please open an issue on the [SQLite RAG GitHub repository](https://github.com/sqliteai/sqlite-rag/issues).
