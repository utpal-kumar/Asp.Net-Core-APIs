# Asp.Net-Core REST APIs

This is a perfect sample of how to implement REST APIs in asp.net core.

Here I have learned how to get resource by REST API. I have also learned how to apply paging, order by, filter and shaping data as requested.
The "X-Pagination" header is used for representing pagination metadata. 

            var paginationMetadata = new
            {
                totalCount = authorsFromRepo.TotalCount,
                pageSize = authorsFromRepo.PageSize,
                currentPage = authorsFromRepo.CurrentPage,
                totalPages = authorsFromRepo.TotalPages,
                previousPageLink = previousPageLink,
                nextPageLink = nextPageLink
            };

I have learned when to use HTTP POST, PUT and PATCH requests. I have learned by PATH request updating object can be very faster and safely. 
Perfect model mapping and validations are done by asp.net core provided return types e.g. BadRequest, NotFound etc

            if (!_typeHelperService.TypeHasProperties<AuthorDto>
              (fields))
            {
                return **BadRequest**();
            }

            var authorFromRepo = _libraryRepository.GetAuthor(id);

            if (authorFromRepo == null)
            {
                return **NotFound**();
            }

This sample shows how to use dummy data for entity framework.

The BooksController is a sample that shows how to add, update and delete single child and collection of child objects.
