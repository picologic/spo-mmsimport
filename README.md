# SharePoint Online - Managed Metadata Import Utility
Simple console application for importing terms with custom properties into the Managed Metadata store in SharePoint Online/O365.

### Please Note
This is an extremely simple utility in its design and development. There are many things it does not do. It may be expanded in the future with additional capabilities but at this time it is in a fairly rudimentary state.

# Usage
The application expects a simple CSV file input with the following format:
```
Term,Prop_Custom1,Prop_Custom2,Prop_CustomN
Product A,Foo,Bar,Baz
Product B,Abc,Def,Ghi
```

Given that input, the utility will create terms in the term store using the Term column. Each column that starts with the prefix "Prop_" will be imported as custom properties on the term (the Prop_ prefix will be stripped before creating the property).  So for the input above, the following terms would be created:

* Product A (Custom1 = Foo, Custom2 = Bar, CustomN = Baz)
* Product B (Custom1 = Abc, Custom2 = Def, CustomN = Ghi)

### Import Command
This command imports the terms in a CSV file into the term set specified.

```
SPOMMSImport.exe import -f c:\myterms.csv --url https://mycompany.sharepoint.com/sites/mysite -u foo@mycompany.com -p 12345 --termGroup "Company Terms" --termSet "Products"
```

#### Arguments
The command expects the following arguments:

```
  -f, --file         Required. Data file containing data to import.

  -l, --url          Required. Url to the site or tenant

  -u, --username     Required. Username to authenticate

  -p, --password     Required. Password to authenticate

  -t, --termStore    Term store to import to.  If empty the default site 
                     collection term store will be used

  -g, --termGroup    Required. Term group containing the term set to import to.

  -s, --termSet      Required. Term set to import terms into.
```

### Clear Command
This command clears all terms in the term set specified.

```
SPOMMSImport.exe clear --url https://mycompany.sharepoint.com/sites/mysite -u foo@mycompany.com -p 12345 --termGroup "Company Terms" --termSet "Products"
```

#### Arguments
The command expects the following arguments:

```
  -l, --url          Required. Url to the site or tenant

  -u, --username     Required. Username to authenticate

  -p, --password     Required. Password to authenticate

  -t, --termStore    Term store to import to.  If empty the default site 
                     collection term store will be used

  -g, --termGroup    Required. Term group containing the term set to import to.

  -s, --termSet      Required. Term set to import terms into.
```

# Limitations
Currently the utility does not support the following:

* Nested terms - in the future this utility should be expanded to support a CSV format identical to the one you can upload through the web UI which supports nesting and some additional properties.
* Updating existing terms with new properties - currently if a term already exists it will be skipped
* Better performance - there are many operations that could be batched together to improve performance
