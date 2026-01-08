# Globus Data Transfer Script

This python script automates batched data transfers of .egg files between Globus endpoints using the Globus CLI. It is designed for large, distributed datasets where files are organized across many directories and transfers must be staged, monitored, and manually gated for safety.

## Description

This script searches for directories using Globus Search, then transfers all `.egg` files from those directories to a destination endpoint in batches. It processes transfers in configurable batch sizes with manual confirmation between batches.

## Prerequisites

- Python 3.x
- [Globus CLI](https://docs.globus.org/cli/) installed and configured
- Active Globus authentication with access to source and destination endpoints

## Configuration

Before running the script, update the following constants in [transferGlobusData.py](transferGlobusData.py):

- `searchUuid`: The UUID of your Globus Search index
- `searchQuery`: Query string to find directories (default: "asiagenawi-data")
- `sourceUuid`: UUID of the source Globus endpoint
- `destUuid`: UUID of the destination Globus endpoint
- `batchSize`: Number of directories to process before pausing (default: 10)

## How It Works

1. **Search**: Queries Globus Search to find directories matching the search query
2. **Extract**: Parses search results to extract directory paths and names
3. **Batch Process**: Processes directories in batches of the specified size
4. **Transfer**: For each directory, lists all files and transfers only `.egg` files
5. **Wait**: Waits for all transfers in a batch to complete before proceeding
6. **Pause**: Prompts for user input before processing the next batch

## Usage

```bash
python transferGlobusData.py
```

The script will:
1. Execute the search query and save results to `output.json`
2. Process directories in batches
3. Transfer all `.egg` files from each directory
4. Wait for all transfers in the batch to complete
5. Pause and wait for you to press Enter before continuing to the next batch

## Output

- `output.json`: Contains the raw search results from Globus Search

## Notes

- The script only transfers files with the `.egg` extension
- Manual confirmation (press Enter) is required between batches
- Transfer task IDs are tracked and the script waits for completion before proceeding
- Source paths are automatically adjusted by removing the first path component
