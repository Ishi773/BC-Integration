# BC-Integration

Table of Contents
1. GraphQL Query (Pimcore)
2. Transformation (Mapping Pimcore to Omnium)
2.1. Field Mapping Overview
2.2. Details on properties

1. GraphQL Query (Pimcore)

query = """
    {
        getProduct(id: %d) {
            id
            AlternateProductNumber
            EAN
            ProductType
            CalcMagentoName
            ReleaseYear
            SalesPrice
            SpecialPrice
            SpecialPriceFromDate
            SpecialPriceToDate
            StockStatusDate
            SalesDate
            MinPurchQtyND
            MinPurchQtyStore
            AssortmentControl
            Suppliers {
                element {
                    id
                    Name
                    ApprovedForMagento
                    AXonly
                    OAId
                    SupplierId
                }
            }
            SupplierSKU
            LeadTimeCalc
            Currency
            PurchasePriceSupplier
            StatusCalculated
            Length
            Width
            Height
            Weight
            MarketCategory {
                element {
                    Name
                }
                metadata {
                    name
                    value
                }
            }
            BookSeries {
                element {
                    Name
                }
            }
            Publisher {
                ... on object_Publisher {
                    Name
                }
            }
            Brand {
                ... on object_Brand {
                    Name
                    BrandDescription
                }
            }
            GenreAndForm {
                element {
                    Name
                    Code
                    CategoryType
                }
            }
            MainProductGroup {
                ... on object_ProductGroup {
                    Name
                    Code
                    parent {
                        ... on object_ProductGroup {
                            Name
                            Code
                            parent {
                                ... on object_ProductGroup {
                                    Name
                                    Code
                                }
                            }
                        }
                    }
                }
            }
            BookGroup {
                element {
                    ... on object_BookGroup {
                        Code
                        Name
                        parent {
                            ... on object_BookGroup {
                                Code
                                Name
                                parent {
                                    ... on object_BookGroup {
                                        Code
                                        Name
                                    }
                                }
                            }
                        }
                    }
                }
            }
            ProductOwner {
                ... on object_ProductOwner {
                    Name
                }
            }
            BookBinding
            BookLanguage
            Contributor {
                element {
                    Name
                    InvertedName
                    ContributorID
                }
                metadata {
                    value
                }
            }
            ThemaCodes {
                element {
                    Code
                    Name
                    CategoryType
                }
            }
        }
    }
    
2. Transformation (Mapping Pimcore to Business Central)




| Pimcore GQL Field                     | BC Field                        | Notes                                           |
|---------------------------------------|---------------------------------|-------------------------------------------------|
| no                                    | number                          | Item identifier                                 |
| name                                  | displayName                     | Item description/name                           |
| assortmentCode                        | assortmentCode                  | Assortment code (if supported by BC)            |
| orderMultipleStore                    | orderMultiple                   | Store order multiple (BC uses single field)     |
| orderMultipleWH                       | minimumOrderQuantity            | Warehouse order multiple                        |
| releaseDate                           | releaseDate                     | Release date                                    |
| salesDate                             | salesDate                       | Sales start date                                |
| stockStatusDate                       | stockStatusDate                 | Stock status update date                        |
| division                              | divisionCode                    | Division code (if BC supports)                  |
| itemCategory                          | itemCategoryCode                | Item category code                              |
| itemCategoryDescription               | itemCategoryDescription         | Description (optional)                          |
| productGroup                          | productGroupCode                | Product group code (if supported)               |
| brand                                 | brand                           | Brand name (if BC supports)                     |
| productType                           | type                            | Avoid type—use itemCategoryCode instead         |
| itemUOM[0].code                       | baseUnitOfMeasure.code          | Unit of measure code                            |
| barcodes[0].ean                       | gtin                            | Global trade item number                        |
| vendors[0].vendorNo                   | vendor.vendorId                 | Vendor ID                                       |
| vendors[0].vendorItemNo               | vendor.vendorItemNumber         | Vendor item number                              |
| vendors[0].leadTime                   | vendor.leadTimeCalculation      | Lead time (days)                                |
| purchasePrices[0].directUnitCost      | unitCost                        | Purchase unit cost                              |
| salesPrices[0].unitPriceInclVAT       | unitPrice                       | Sales unit price (incl. VAT)                    |



Formatting that needs to be done while mapping

