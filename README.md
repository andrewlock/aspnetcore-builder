# A generalised ASP.NET Core builder Docker image

This repo contains a [Dockerfile](/Dockerfile) that can be used to build ASP.NET Core images where they conform to a standard layout:

* Should have a single _.sln_ file in the root folder
* Should have all library and app projects in a _src_ subdirectory
* Should have all test projects in a _test_ subdirectory

The image uses Docker's `ONBUILD` command to execute the following in your project's directory:

1. Copy the _.sln_ file and _NuGet.config_
1. Copy the _.csproj_ files into the image and run `dotnet restore` (to [take advantage of Docker's layer caching mechanism](https://andrewlock.net/optimising-asp-net-core-apps-in-docker-avoiding-manually-copying-csproj-files/))
1. Set the version number to the value of the `BUILD_VERSION` argument (passed view `--build-args`, for example)
1. Copy the source code to the builder image and run `dotnet build`
1. Run `dotnet test` on every project in the _test_ folder
