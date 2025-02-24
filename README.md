# 🔍 Azure Resource Graph Query Collection

Hey there! Welcome to my collection of Azure Resource Graph queries. This repo is basically my personal stash of useful KQL (Kusto Query Language) queries that I use to dig through Azure resources.

## 📁 What's Inside?

Each folder contains a KQL query along with its own README that breaks down:
- What the query actually does
- How it works step by step
- What the output looks like
- Some sample results to give you an idea

## 💡 How To Run These Queries

### 🐚 PowerShell
#### Single Query
1. Clone repository

```PowerShell
git clone https://github.com/juddlestone/resource-graph-queries
```

2. Move into the directory

```PowerShell
cd azure-resource-graph-queries
```

3. Install required modules

```PowerShell
Install-Module -Name Az.ResourceGraph
```

4. Get query of choice

```PowerShell
$query = Get-Content -Path ".\orphaned-disks\query.kql" -Raw
```

5. Execute query

```PowerShell
$results = Search-AzGraph -Query $query
```

6. Export results

```PowerShell
$results | Export-Csv -Path ".\results.csv" -NoTypeInformation
```

####  All Queries
1. Amend step four to the following

```PowerShell
$queryFiles = Get-ChildItem -Path "." -Filter "query.kql" -Recurse
```

2. Iterate over each file

```PowerShell
foreach ($file in $queryFiles) {
    Write-Host "Running query from: $($file.FullName)"
    $queryContent = Get-Content -Path $file.FullName -Raw
    $results = Search-AzGraph -Query $queryContent
    
    $outputPath = Join-Path $file.DirectoryName "results.csv"
    $results | Export-Csv -Path $outputPath -NoTypeInformation
    Write-Host "Results saved to: $outputPath`n"
}
```

### 🖥️ Portal
1. Pop open Azure Portal
2. Head to Resource Graph Explorer
3. Copy & paste any query you fancy
4. Hit run and see what you find!


