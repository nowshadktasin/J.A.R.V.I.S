import os
import json
from pathlib import Path

# File paths based on the names seen in the GitHub screenshot
file_names = [
    "Calendar Agent.json",
    "Contact Agent.json",
    "Email Agent.json",
    "J.A.R.V.I.S Main Agent.json",
    "Research Agent.json"
]

# Base directory where uploaded files are likely stored
base_dir = "/mnt/data"

# Dictionary to hold all workflows
combined_workflows = {}

# Load and combine JSON contents
for file_name in file_names:
    file_path = Path(base_dir) / file_name
    if file_path.exists():
        with open(file_path, "r", encoding="utf-8") as f:
            try:
                content = json.load(f)
                key = file_name.replace(".json", "").replace(" ", "_").lower()
                combined_workflows[key] = content
            except json.JSONDecodeError:
                combined_workflows[file_name] = {"error": "Invalid JSON format"}

# Save combined JSON to a single file
output_path = Path(base_dir) / "combined_workflows.json"
with open(output_path, "w", encoding="utf-8") as f:
    json.dump(combined_workflows, f, indent=2)

output_path.name  # return just the filename for display

