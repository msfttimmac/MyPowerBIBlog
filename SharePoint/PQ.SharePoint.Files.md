

***

# SharePoint.Files

## Purpose

Retrieves a list of **files** stored in a SharePoint site, specifically from document libraries.

## Function Signature

```m
SharePoint.Files(url as text, optional options as nullable record) as table
```

## What It Returns

A table containing metadata about files, such as:

*   File name
*   File path
*   File size
*   Date modified

## Use Case

Use this when you want to work with **individual files**, such as:

*   Excel
*   CSV
*   Other file types stored in SharePoint document libraries

## Behavior

*   Focuses on **file-level access**
*   Allows drilling into specific folders or libraries using the URL
*   Does **not** provide access to:
    *   SharePoint lists
    *   Site metadata
    *   Pages or web parts

## Example

```m
SharePoint.Files(
    "https://contoso.sharepoint.com/sites/MySite",
    [ApiVersion = 15]
)
```

> Retrieves a table of all files in the document libraries of the specified SharePoint site.

## Common Scenarios

*   Importing Excel or CSV files from a SharePoint document library
*   Accessing files in a specific folder within a document library

## Limitations

*   Limited to **files only**
*   Cannot access:
    *   SharePoint lists
    *   Site pages or other non-file content
*   Requires a precise URL to the site or folder containing the files

## Documentation

*   **SharePoint.Files** – PowerQuery M | Microsoft Learn

***

# SharePoint.Contents

## Purpose

Retrieves a broader view of the **contents** of a SharePoint site, including:

*   Files
*   Folders
*   Document libraries
*   Potentially lists and subsites

## Function Signature

```m
SharePoint.Contents(url as text, optional options as nullable record) as table
```

## What It Returns

A table listing site contents such as:

*   Document libraries
*   Folders
*   Lists
*   Subsites

(Returned content depends on permissions and API version.)

## Use Case

Use this when you need to:

*   Explore the **structure of a SharePoint site**
*   Access a mix of files, folders, and potentially lists

## Behavior

*   Provides a **hierarchical view** of the site
*   Enables navigation through folders and document libraries
*   May include lists if supported by the API version

## Example

```m
SharePoint.Contents(
    "https://contoso.sharepoint.com/sites/MySite",
    [ApiVersion = 15]
)
```

> Retrieves all top-level content in the SharePoint site (libraries, folders, lists, subsites).

## Common Scenarios

*   Exploring site structure to locate libraries or folders
*   Navigating to a specific document library
*   Accessing mixed content (files and folders)

## Limitations

*   May return more data than necessary if you only need files
*   Accessing SharePoint lists may require:
    *   Additional steps, or
    *   `SharePoint.Tables` specifically for lists

## Documentation

*   **SharePoint.Contents** – PowerQuery M | Microsoft Learn

***

# Key Differences

| Aspect      | SharePoint.Files                   | SharePoint.Contents                        |
| ----------- | ---------------------------------- | ------------------------------------------ |
| Scope       | Files only (Excel, CSV, PDF, etc.) | Files, folders, libraries, lists, subsites |
| Output      | File metadata table                | Table of site contents                     |
| Use Case    | Importing specific files           | Exploring site structure or mixed content  |
| Granularity | File-level access                  | Site / folder-level access                 |
| Performance | Faster for file retrieval          | Slower for large or complex sites          |
| Navigation  | Limited to files at the URL        | Full hierarchical navigation               |

***

# ApiVersion Parameter

*   Both functions support the optional parameter:
    ```m
    [ApiVersion = 15]
    ```
*   API version **15** is typically used for:
    *   SharePoint Online
    *   Office 365
*   Ensures compatibility with SharePoint Online REST APIs
*   If omitted, Power BI uses the default API version for the environment

***

# Practical Examples

## Import a Specific Excel File

Use `SharePoint.Files` and filter by file name.

```m
let
    Source = SharePoint.Files(
        "https://contoso.sharepoint.com/sites/MySite/Shared Documents",
        [ApiVersion = 15]
    ),
    Filtered = Table.SelectRows(
        Source,
        each [Name] = "MyData.xlsx"
    )
in
    Filtered
```

***

## Explore a Site and Locate a Folder

Use `SharePoint.Contents` to navigate the site structure.

```m
let
    Source = SharePoint.Contents(
        "https://contoso.sharepoint.com/sites/MySite",
        [ApiVersion = 15]
    ),
    Documents = Source{[Name="Shared Documents"]}[Content],
    MyFolder = Documents{[Name="MyFolder"]}[Content]
in
    MyFolder
```

***

# When to Use Which

*   ✅ **Use `SharePoint.Files`** when:
    *   You only need files
    *   You know the library or folder
    *   Performance matters

*   ✅ **Use `SharePoint.Contents`** when:
    *   You need to explore the site
    *   You don’t know the exact path yet
    *   You’re navigating through folders or libraries

***

# Notes

*   **Authentication**
    *   Requires organizational or SharePoint credentials in Power BI

*   **Performance**
    *   `SharePoint.Files` → better for targeted file access
    *   `SharePoint.Contents` → slower on large sites due to breadth

*   **SharePoint Lists**
    *   Use `SharePoint.Tables` for lists (not files)

Below is a **ready-to-paste Markdown section** you can append to your existing content. I’ve split it exactly as requested and only included **authoritative Microsoft Learn links** plus **high‑quality third‑party + YouTube resources** that explicitly cover **SharePoint.Files vs SharePoint.Contents in Power Query**.

***

## Microsoft Learn Documentation

Official Microsoft documentation covering the Power Query M functions and SharePoint connectors discussed above:

*   **SharePoint.Files – Power Query M function reference**  
    Official reference detailing syntax, parameters (including `ApiVersion`), behavior, and return schema.  
    [Microsoft Learn – SharePoint.Files](https://learn.microsoft.com/en-us/powerquery-m/sharepoint-files)

*   **SharePoint.Contents – Power Query M function reference**  
    Official reference describing hierarchical navigation, connector behavior, `ApiVersion`, and implementation options.  
    [Microsoft Learn – SharePoint.Contents](https://learn.microsoft.com/en-us/powerquery-m/sharepoint-contents)

*   **SharePoint Folder connector in Power Query**  
    Describes capabilities, authentication options, site URL requirements, and supported experiences (Excel, Power BI, Fabric).  
    [Microsoft Learn – SharePoint Folder connector](https://learn.microsoft.com/en-us/power-query/connectors/sharepoint-folder)

*   **Power Query documentation (landing page)**  
    Central hub for Power Query concepts, connectors, best practices, and advanced topics.  
    [Microsoft Learn – Power Query documentation](https://learn.microsoft.com/en-us/power-query/)

***

## Additional Documentation

### Third‑Party Articles & Blogs

*   **SharePoint.Files vs SharePoint.Contents – Which one should you use?**  
    Deep technical comparison covering performance implications, navigation behavior, and common pitfalls at scale.  
    [Excelized – SharePoint.Files vs SharePoint.Contents](https://www.excelized.de/post/sharepoint-files-vs-sharepoint-contents-in-power-query-which-one-should-you-use)

*   **Show all site content when importing from SharePoint in Power Query**  
    Practical walkthrough explaining why files may be missing when using `SharePoint.Files` and when switching to `SharePoint.Contents` is necessary.  
    [Wise Owl Training – Show all site content](https://www.wiseowl.co.uk/excel/blogs/using-excel/data/show-sharepoint-site-contents/)

*   **Get data from OneDrive or SharePoint with Power Query**  
    Explains correct URL discovery, authentication behavior, and common connection errors when working with SharePoint-hosted files.  
    [Excel Off The Grid – Power Query & SharePoint](https://exceloffthegrid.com/power-query-connect-onedrive-sharepoint/)

***

### YouTube Videos

*   **SharePoint Files or Folder Contents in Power BI using Power Query** (RADACAD)  
    Clear visual explanation of the differences between `SharePoint.Files` and `SharePoint.Contents`, with real‑world scenarios.  
    [YouTube – RADACAD](https://www.youtube.com/watch?v=lBKqGUT0gvQ)

*   **Power BI – Get Data from SharePoint Folder using Power Query**  
    Demonstrates importing and combining files from SharePoint using Power Query, including performance considerations.  
    [YouTube – SharePoint Folder with Power Query](https://www.youtube.com/watch?v=1hWtd-WxgNI)

*   **How to connect Power Query to SharePoint quickly and easily**  
    Covers connecting to individual files vs folders, proper URL usage, and use of `SharePoint.Contents`.  
    [YouTube – Excel y Finanzas](https://www.youtube.com/watch?v=Sch8ixVS8qg)

***
