# PowerShell File Organization Procedures

## Introduction: A Humanistic Approach to File Organization

File organization is not merely a technical practice but a foundation for meaningful knowledge management and collaboration. This document embraces a humanistic approach to file organization through PowerShell, recognizing that well-structured information environments can:

- Create cognitive bridges between disciplines and individuals
- Foster engagement through invitational frameworks
- Support human-centered learning and knowledge exchange

By organizing files in ways that reflect human thought patterns and collaborative needs, we can transform technical systems into environments that invite exploration, connection, and understanding. The following PowerShell procedures support this approach through contextual organization, bridging different knowledge domains, and creating systems that adapt to diverse cognitive approaches.

## Directory Management

### Creating Directories
```powershell
# Create single directory
New-Item -ItemType Directory -Path "folder_name"

# Create multiple directories
New-Item -ItemType Directory -Path "folder1", "folder2", "folder3"

# Create nested directories
New-Item -ItemType Directory -Path "parent/child" -Force
```

## File Operations

### Basic File Operations
```powershell
# Copy a file
Copy-Item -Path "source.txt" -Destination "destination.txt"

# Move a file
Move-Item -Path "source.txt" -Destination "folder/destination.txt"

# Delete a file
Remove-Item -Path "file.txt"
```

### Listing Files and Directories
```powershell
# List all items in current directory
Get-ChildItem

# List with formatted output
Get-ChildItem -Force | Format-Table Name

# List files only
Get-ChildItem -File

# List directories only
Get-ChildItem -Directory
```

## Handling Files with Special Characters

### Using Variable References
```powershell
# Store file path in variable
$file = Get-ChildItem -Path "File with special characters.txt"
Copy-Item -Path $file.FullName -Destination "destination/folder/"
```

### Pattern Matching Approach
```powershell
# Find and operate on files matching a pattern
Get-ChildItem | Where-Object {$_.Name -match "pattern"} | 
    ForEach-Object { 
        Copy-Item $_.FullName -Destination "destination/folder/newname.txt"
        Remove-Item $_.FullName 
    }
```

### Using CMD as Fallback
```powershell
# When PowerShell has issues with special characters, use CMD
cmd /c copy "File with special characters.txt" "destination\folder\newname.txt"
cmd /c del "File with special characters.txt"
```

### Copy-and-Delete Pattern
```powershell
# Copy to a new name and then delete original
Copy-Item "Complex file name.txt" -Destination "destination\simpler-name.txt"
Remove-Item "Complex file name.txt"
```

### Handling Unicode and Special Characters
```powershell
# Use wildcards with Get-ChildItem to match files with problematic characters
Get-ChildItem -Path "*section*" | ForEach-Object {
    Copy-Item $_.FullName -Destination "destination\normalized-name.txt"
    Remove-Item $_.FullName
}

# Normalize Unicode characters before operations
$fileName = "File with ‑ special ‑ characters.txt"
$normalizedName = $fileName -replace '[^\p{L}\p{N}\.\-_\s]', '_'
Rename-Item -Path $fileName -NewName $normalizedName

# Use -LiteralPath for exact file names with special characters
Copy-Item -LiteralPath "File[with]brackets.txt" -Destination "destination\"
```

## Organizing Files by Content

### Creating an Organization Structure
```powershell
# Create a logical folder structure
New-Item -ItemType Directory -Path "documents", "images", "data"

# Move files based on extension
Get-ChildItem -Filter "*.docx" | Move-Item -Destination "documents/"
Get-ChildItem -Filter "*.jpg" | Move-Item -Destination "images/"
Get-ChildItem -Filter "*.csv" | Move-Item -Destination "data/"
```

### Organization by Content Pattern
```powershell
# Move files containing a keyword in their content
Get-ChildItem -Filter "*.txt" | 
    Where-Object { Get-Content $_.FullName | Select-String -Pattern "keyword" } |
    Move-Item -Destination "relevant-folder/"
```

## Human-Centered File Organization

### Context-Based Organization
```powershell
# Create folders based on project contexts
New-Item -ItemType Directory -Path "research", "implementation", "documentation"

# Organize files into context-relevant folders
Get-ChildItem -Filter "*research*" | Move-Item -Destination "research/"
Get-ChildItem -Filter "*impl*" | Move-Item -Destination "implementation/"
Get-ChildItem -Filter "*doc*" | Move-Item -Destination "documentation/"
```

### Cognitive Bridge Patterns
```powershell
# Create symlinks to build connections between related files in different contexts
New-Item -ItemType SymbolicLink -Path "research/implementation-reference" -Target "../implementation"

# Tag files with metadata for cross-referencing
$files = Get-ChildItem -Filter "*.md"
foreach ($file in $files) {
    $content = Get-Content $file.FullName
    $newContent = "---`ntags: [research, cognitive-bridge]`n---`n" + $content
    Set-Content -Path $file.FullName -Value $newContent
}
```

### Creating Invitational File Structures
```powershell
# Create README files in each directory to explain purpose and invite engagement
$directories = Get-ChildItem -Directory
foreach ($dir in $directories) {
    $readmePath = Join-Path $dir.FullName "README.md"
    $content = "# $($dir.Name)`n`nThis directory contains files related to $($dir.Name) context. Feel free to explore and contribute."
    Set-Content -Path $readmePath -Value $content
}

# Create centralized index for navigation across contexts
$indexContent = "# Project Navigation Index`n`n"
Get-ChildItem -Directory | ForEach-Object {
    $indexContent += "## $($_.Name)`n"
    $indexContent += Get-ChildItem -Path $_.FullName | ForEach-Object { "- [$($_.Name)]($($_.FullName))`n" }
    $indexContent += "`n"
}
Set-Content -Path "index.md" -Value $indexContent
```

## Error Handling Strategies

### Error-aware Operations
```powershell
# Try operation and handle failure
try {
    Move-Item -Path "source.txt" -Destination "folder/destination.txt" -ErrorAction Stop
} catch {
    Write-Host "Error moving file: $_"
    # Alternative approach
    Copy-Item -Path "source.txt" -Destination "folder/destination.txt"
    Remove-Item -Path "source.txt"
}
```

### Verifying Operations
```powershell
# Verify before deleting
$destination = "folder/destination.txt" 
Copy-Item -Path "source.txt" -Destination $destination
if (Test-Path $destination) {
    Remove-Item -Path "source.txt"
}
```

## Conclusion: Bridging Technology and Human Experience

The PowerShell procedures outlined in this document represent more than technical operations—they embody a philosophy where technology serves human understanding and connection. By approaching file organization as an invitation to engage with knowledge in meaningful ways, we create environments where:

1. **Technical systems reflect human thought patterns** - Organization by context rather than arbitrary technical constraints
2. **Bridges form between different knowledge domains** - Creating connections across disciplinary boundaries
3. **Individual cognitive approaches are honored** - Supporting multiple pathways to discover and interact with information

When implemented thoughtfully, these practices transform file systems from mere storage into knowledge landscapes that invite exploration, foster connections, and adapt to individual learning approaches. This humanistic perspective on file organization becomes particularly valuable in educational technology contexts, where the goal is not just efficient data management but creating environments that support meaningful human learning and collaboration. 