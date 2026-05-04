
# Power BI Data Ingestion from ProPublica API using Power Query

## Overview

This guide outlines how to use **Power BI Desktop and Power Query (M language)** to ingest, paginate, and transform nonprofit organization data from the **ProPublica Nonprofit API**. The solution dynamically retrieves all available pages of results, expands nested JSON structures, enriches records with additional API calls, and produces a fully structured dataset ready for reporting and analytics in Power BI. This approach is ideal for scalable API ingestion scenarios where data spans multiple pages and contains hierarchical structures.

***

## Steps

1.  In **Power BI Desktop**, click **Transform Data** to open Power Query
2.  In the ribbon, select **New Source → Blank Query**
3.  Copy and paste the Power Query script below into the query editor
4.  Click **OK** to execute the query

***

## Power Query (M Code)

```powerquery
let
    // Define the query parameters
    SearchQuery = "propublica", // Change this to your desired search term
    BaseUrl = "https://projects.propublica.org/nonprofits/api/v2/search.json?page=",

    // Function to fetch a single page
    GetPage = (PageNumber as number) as record =>
        let
            Url = BaseUrl & Text.From(PageNumber),
            Source = Web.Contents(Url),
            Json = Json.Document(Source),
            Organizations = Json[organizations]
        in
            Json,

    // Fetch the first page to determine total pages
    FirstPage = GetPage(0),
    TotalPages = FirstPage[num_pages],

    // Generate list of page numbers
    PageList = List.Numbers(0, TotalPages, 1),

    // Fetch all pages dynamically
    AllPages = List.Generate(
        () => [Page = 0, Data = GetPage(0)],
        each [Page] < TotalPages,
        each [Page = [Page] + 1, Data = GetPage([Page] + 1)],
        each [Data][organizations]
    ),

    // Combine into a single list
    CombinedOrganizations = List.Combine(AllPages),

    // Convert list to table
    Table = Table.FromList(CombinedOrganizations, Splitter.SplitByNothing(), null, null, ExtraValues.Error),

    // Expand records
    ExpandedTable = Table.ExpandRecordColumn(
        Table,
        "Column1",
        Record.FieldNames(CombinedOrganizations{0}),
        Record.FieldNames(CombinedOrganizations{0})
    ),

    // Set column data types
    FinalTable = Table.TransformColumnTypes(
        ExpandedTable,
        {
            {"ein", type text},
            {"name", type text},
            {"city", type text},
            {"state", type text},
            {"ntee_code", type text},
            {"score", type number}
        },
        "en-US"
    ),

    // Add derived columns
    #"Added Custom" = Table.AddColumn(FinalTable, "guidestar_url", each "https://www.guidestar.org/profile/" & [strein]),

    // Enrich with organization-specific API call
    #"Added Custom1" = Table.AddColumn(
        #"Added Custom",
        "Organization_Information",
        each Json.Document(
            Web.Contents(
                "https://projects.propublica.org/nonprofits/api/v2/organizations/" 
                & Text.From([ein]) 
                & ".json"
            )
        )
    ),

    // Expand nested JSON structures
    #"Expanded Organization_Information1" = Table.ExpandRecordColumn(
        #"Added Custom1",
        "Organization_Information",
        {"organization", "filings_with_data", "filings_without_data", "data_source", "api_version"}
    ),

    #"Expanded Organization_Information.organization" = Table.ExpandRecordColumn(
        #"Expanded Organization_Information1",
        "Organization_Information.organization",
        Record.FieldNames(#"Expanded Organization_Information1"[Organization_Information.organization]{0})
    ),

    #"Expanded Organization_Information.filings_with_data" =
        Table.ExpandListColumn(#"Expanded Organization_Information.organization", "Organization_Information.filings_with_data"),

    #"Expanded Organization_Information.filings_with_data1" =
        Table.ExpandRecordColumn(
            #"Expanded Organization_Information.filings_with_data",
            "Organization_Information.filings_with_data",
            Record.FieldNames(#"Expanded Organization_Information.filings_with_data"[Organization_Information.filings_with_data]{0})
        ),

    #"Expanded Organization_Information.filings_without_data" =
        Table.ExpandListColumn(#"Expanded Organization_Information.filings_with_data1", "Organization_Information.filings_without_data"),

    #"Expanded Organization_Information.filings_without_data1" =
        Table.ExpandRecordColumn(
            #"Expanded Organization_Information.filings_without_data",
            "Organization_Information.filings_without_data",
            {"tax_prd", "tax_prd_yr", "formtype", "formtype_str", "pdf_url"}
        )

in
    #"Expanded Organization_Information.filings_without_data1"
```

***

## Key Capabilities Demonstrated

*   **API Pagination Handling**
    *   Uses `List.Generate` to loop through all available pages dynamically

*   **Dynamic JSON Parsing**
    *   Expands nested arrays and records without hardcoding schema

*   **Data Enrichment**
    *   Performs per-row API calls using EIN to retrieve detailed organization data

*   **Scalable Data Transformation**
    *   Designed for high-volume datasets with consistent transformation patterns

*   **Reusable Pattern**
    *   Applicable to other REST APIs with pagination + nested JSON

***

## Resources

### Microsoft Learn Documentation

*   **Power Query Overview**  
    <https://learn.microsoft.com/power-query/power-query-what-is-power-query>

*   **Power Query M Language (Reference)**  
    <https://learn.microsoft.com/powerquery-m/>

*   **Web.Contents Function**  
    <https://learn.microsoft.com/powerquery-m/web-contents>

*   **Handling JSON in Power Query**  
    <https://learn.microsoft.com/power-query/connectors/json>

*   **Working with Lists, Records, and Tables**  
    <https://learn.microsoft.com/powerquery-m/list-functions>

***

### Additional Documentation

*   **ProPublica Nonprofit API**  
    <https://projects.propublica.org/nonprofits/api>

*   **GuideStar (Candid) Organization Profiles**  
    <https://www.guidestar.org>
