  Base Filter - README

Base Filter
===========

Overview
--------

The `BaseFilter` package allows you to easily filter, sort to your database queries. It handles common query parameters such as filters, sorting, and global searching, which can be used in your API endpoints to manage how data is returned to the client.

Installation
------------

To get started, install the package using `pip`:

    pip install dak0k_base_filter

Once installed, you can import the necessary components to use it in your project.

Importing the BaseFilter
------------------------

    from dak0k_base_filter.dak0k_base_filter import BaseFilter

Usage Example
-------------

Here’s an example of how you can use the `BaseFilter` class within a view to filter, sort, and paginate a queryset:

    
    def list(self, request):
        # Retrieve query parameters
        params = request.query_params
        
        # Fetch all items from the database
        items = Entity.objects.all()
        
        # Initialize BaseFilter with the queryset and parameters
        filter_obj = BaseFilter(queryset=items, params=params)
        
        # Apply filters to the queryset
        filtered_queryset = filter_obj.apply_filters()
        
        pagination = PageNumberAsLimitOffset()
        pages = pagination.paginate_queryset(filtered_queryset, request)
        
        if pages is not None:
            serializer = self.get_serializer_class(pages, many=True)
            return pagination.get_paginated_response(serializer.data)
        
        serializer = self.get_serializer_class(filtered_queryset, many=True)
        return Response(serializer.data)
        

### Parameters Handled

*   **Filters** - The `filters` parameter allows you to filter by various fields like `id`, `name`, or `description`. The filtering logic can be tailored to your needs.
*   **Sorting** - The `sorting` parameter allows you to define how the results should be sorted. You can specify the field and order (ascending or descending).
*   **Global Filter** - The `globalFilter` parameter allows for a global search across all fields in the table. This is useful for searching by any text field or other general criteria.

How to Send a Request
---------------------

To make use of the filters, sorting, and global filter functionality, you can send a request with the following parameters:

### Example Request:

    https://localhost/api/entity/?filters=[{"id":"Name","value":"John"}]&sorting=[{"id":"Name","desc":true}]&globalFilter=john

### Parameters Explained:

*   **filters** - The `filters` parameter is a list of dictionaries where you can specify the field (`id`) to filter by and the corresponding `value`. This is useful for filtering data by name, ID, description, or any other field.  
    Example: `filters=[{"id":"Name","value":"John"}]` filters results where the "Name" field contains "John".
*   **sorting** - The `sorting` parameter defines the field to sort by and whether it should be in ascending (`true`) or descending (`false`) order.  
    Example: `sorting=[{"id":"Name","desc":true}]` sorts the results by the "Name" field in descending order.
*   **globalFilter** - The `globalFilter` parameter allows a general search across all fields in the database table.  
    Example: `globalFilter=john` will return results that match "john" in any searchable field.

Additional Features
-------------------

### Custom Filters

You can extend the `BaseFilter` class to create custom filters for your project. This allows for a more tailored approach depending on your business logic.

Conclusion
----------

With the `BaseFilter` package, you can easily manage common query parameters such as filters, sorting, globalFilter for your APIs, allowing you to provide an efficient and flexible way to retrieve and display data.
