# surf-mcp

A FastMCP-based service that provides tide information using the Storm Glass API.

## Features

- Fetch tide information for any location using latitude and longitude
- Support for date-specific tide queries
- Detailed tide data including high/low tides and station information
- Automatic time zone handling (UTC)

## Prerequisites

- Python 3.x
- Storm Glass API key

## Getting Your Storm Glass API Key

1. Visit [Storm Glass](https://stormglass.io/)
2. Click "Try for Free" or "Sign In" to create an account
3. Once registered, you'll receive your API key

Note on API Usage Limits:
- Free tier: 10 requests per day
- Paid plans available:
  - Small: 500 requests/day (€19/month)
  - Medium: 5000 requests/day (€49/month)
  - Large: 25,000 requests/day (€129/month)
  - Enterprise: Custom plans available

Choose a plan based on your usage requirements. The free tier is suitable for testing and personal use.

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/surf-mcp.git
cd surf-mcp
```

2. Set up your environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Create a `.env` file in the root directory and add your Storm Glass API key:
```
STORMGLASS_API_KEY=your_api_key_here
```

## Usage

The service provides a FastMCP tool for getting tide information:

```python
@mcp.tool()
async def get_tides(latitude: float, longitude: float, date: str) -> str:
    """Get tide information for a specific location and date."""
```

### Parameters:
- `latitude`: Float value representing the location's latitude
- `longitude`: Float value representing the location's longitude
- `date`: Date string in YYYY-MM-DD format

### Example Response:
```
Tide Times:
Time: 2024-01-20T00:30:00+00:00 (UTC)
Type: HIGH tide
Height: 1.52m

Time: 2024-01-20T06:45:00+00:00 (UTC)
Type: LOW tide
Height: 0.25m

Station Information:
Name: Sample Station
Distance: 20.5km from requested location
```

## Running the Server

To start the server:

```bash
python -m src.surf.server
```

## Configure as MCP Server

To add this tool as an MCP server, you'll need to modify your Claude desktop configuration file. The location of this file depends on your operating system:

- MacOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%/Claude/claude_desktop_config.json`

Add the following configuration to your JSON file:

```json
{
    "surf-mcp": {
        "command": "uv",
        "args": [
            "--directory",
            "/Users/yourusername/Code/surf-mcp",
            "run",
            "surf-mcp"
        ],
        "env": {
            "STORMGLASS_API_KEY": "your_api_key_here"
        }
    }
}
```

Note: Replace `your_api_key_here` with your actual Storm Glass API key, and adjust the directory path to match your local installation.

## Error Handling

The service includes robust error handling for:
- API request failures
- Invalid coordinates
- Missing or invalid API keys
- Network timeouts

