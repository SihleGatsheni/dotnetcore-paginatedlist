# dotnetcore-paginatedlist


##How to use the library

## 1. Controller to apply paging and sorting
   public async Task<IActionResult> Index(bool sort,bool shuffle, bool reverse,int pageNumber =1) 
    {
        var productList = await PaginatedList<Product>.CreateAsync(repo.getAll(), pageNumber, 8);
        if (sort)
        {
            productList.orderByAscending();
        }

        if (reverse)
        {
            productList.orderByDescending();
        }

        if (shuffle)
        {
            productList.shuffle();
        }
        return View(productList);
    }
    
    ## 2. Views to display next,previous and current page/index
        @if (Model.TotalPages > 0)
                      {
                          <ul class="pagination justify-content-start" style="margin-top: 10px">
                              @if (Model.PageIndex > 1)
                              {
                                  <li class="page-item">
                                      <a class="page-link" asp-controller="Admin" asp-action="getOrders" asp-route-pageNumber="1">First</a>
                                  </li>
                                  <li>
                                      <a class="page-link" asp-controller="Admin" asp-action="getOrders" asp-route-pageNumber="@(Model.PageIndex -                                             1)">Previous</a>
                                  </li>
                              }
                              @for (var pge = Model.PageIndex; pge <= Model.Count; pge++)
                              {
                                  <li class="page-item @(pge == Model.PageIndex ? "active" : "")">
                                      <a class="page-link" asp-controller="Admin" asp-action="getOrders" asp-route-pageNumber="@pge">@pge</a>
                                  </li>
                              }
                              @if (Model.PageIndex < Model.TotalPages)
                              {
                                  <li class="page-item">
                                      <a class="page-link" asp-controller="Admin" asp-action="getOrders" asp-route-pageNumber="@(Model.PageIndex +                                            1)">Next</a>
                                  </li>
                                  <li>
                                      <a class="page-link" asp-controller="Admin" asp-action="getOrders" asp-route-                                        pageNumber="@(Model.TotalPages)">Last</a>
                                  </li>
                              }
                          </ul>
                    }
                    
                    
## 3. Display sorting buttons
<ul class="pagination justify-content-start" style="margin-top: 10px">
                                <li class="page-item">
                                    <a style="width: 70px;margin-left: 5px;padding-top: 5px" class="page-link" value="sort" asp-action="getOrders" asp-route-sort="@(true)"><i class="fa-solid fa-sort-up text-center"></i></a>
                                </li>
                                <li>
                                    <a style="width: 70px;margin-left: 2px;padding-top: 5px" class="page-link" value="shuffle" asp-action="getOrders" asp-route-reverse="@(true)"><i class="fa-solid fa-sort-down text-center"></i></a>
                                </li>
                                <li>
                                    <a style="width: 70px;margin-left: 2px;padding-top: 5px" class="page-link" value="shuffle" asp-action="getOrders" asp-route-shuffle="@(true)"><i class="fa-solid fa-shuffle text-center"></i></a>
                                </li>
                            </ul>
                    
