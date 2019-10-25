#Steps to create project

```
. <(wget -q -O - https://aka.ms/build-web-api-net-core-setup)
// create the directory structure
dotnet new webapi -o contoso-pets/src/ContosoPets.Api
// build project and redirects .net run output to ContosoPets.Api.log and runs app in background
cd ~/contoso-pets/src/ContosoPets.Api && code .
dotnet build
dotnet ./bin/Debug/netcoreapp3.0/ContosoPets.Api.dll > ContosoPets.Api.log &
//
curl -k -s https://localhost:5001/weatherforecast | jq
// kill all processess of the .net core app
kill $(pidof dotnet)
```

### in Memory DB
`dotnet add package Microsoft.EntityFrameworkCore.InMemory`

# more steps
```
mkdir Models && touch $_/Product.cs
mkdir Data && touch $_/ContosoPetsContext.cs $_/SeedData.cs
```
this added to ConfigureServices method in Startup.cs: 

services.AddDbContext<ContosoPetsContext>(options =>
        options.UseInMemoryDatabase("ContosoPets"));
  
```  
touch ./Controllers/ProductsController.cs
dotnet build --no-restore  //use because no new NuGet packages were added
```

send a post request: 
```
curl -i -k \
    -H "Content-Type: application/json" \
    -d "{\"name\":\"Plush Squirrel\",\"price\":0.01}" \
    https://localhost:5001/products
```

get request:
`curl -k -s https://localhost:5001/products/3 | jq`

put request:
```
curl -i -k \
    -X PUT \
    -H "Content-Type: application/json" \
    -d "{\"id\":2,\"name\":\"Knotted Rope\",\"price\":14.99}" \
    https://localhost:5001/products/2
```
delete request:
`curl -i -k -X DELETE https://localhost:5001/products/1`

get request:
`curl -k -s https://localhost:5001/products | jq`

