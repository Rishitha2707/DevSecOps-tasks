**🚀 .NET Application — Clone, Build & Deploy Using Multi-Stage Dockerfile**

This documentation explains how to build and run a .NET Core application using Docker, where the Dockerfile itself handles:

✅ Cloning the source code from GitHub

✅ Restoring dependencies

✅ Building the application

✅ Publishing it

✅ Running it in a lightweight runtime container


**📁 Project Structure**

Your GitHub repository should contain:
```
.Net-code/
 ├── Program.cs
 └── dotnet-new-demo.csproj
```

**🐳 Multi-Stage Dockerfile (Clone + Build + Deploy)**

Below is the multi-stage Dockerfile used for cloning, building, and deploying the .NET application.

```
# -------- Stage 1: Base Build Image --------
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

WORKDIR /source

# Install git (needed to clone repo inside container)
RUN apt-get update && apt-get install -y git

# Clone your GitHub .NET project
RUN git clone https://github.com/Rishitha2707/.Net-code.git

# Move into cloned directory
WORKDIR /source/.Net-code

# Restore packages
RUN dotnet restore

# Build the application
RUN dotnet build -c Release

# Publish the application (output goes to /app/publish)
RUN dotnet publish -c Release -o /app/publish


# -------- Stage 2: Runtime Image --------
FROM mcr.microsoft.com/dotnet/runtime:7.0

WORKDIR /app

# Copy ONLY the published output from build stage
COPY --from=build /app/publish .

# Expose port
EXPOSE 8080

# Run the application
ENTRYPOINT ["dotnet", "dotnet-new-demo.dll"]
```


**🧪 Testing the Docker Image**

1️⃣ Build the Image
Run the following command from the directory where your Dockerfile exists:
```
docker build -t dotnet-demo-app .
```

<img width="1057" height="74" alt="Screenshot 2025-11-24 113749" src="https://github.com/user-attachments/assets/7f651cdf-7863-4fd4-9dc2-df4ae8a0c651" />


This performs:

✔ Cloning repository

✔ Restoring dependencies

✔ Building code

✔ Publishing

✔ Packaging final runtime image



2️⃣ Run the Container
```
docker run -d -p 8080:8080 dotnet-demo-app
```

<img width="1462" height="83" alt="Screenshot 2025-11-24 113849" src="https://github.com/user-attachments/assets/c74173ab-fa15-4b24-9a04-bbfabe57697a" />


3️⃣ Test the Application
Open a browser and go to:
```
http://<your-server-ip>:8080
```
<img width="1919" height="653" alt="Screenshot 2025-11-24 113908" src="https://github.com/user-attachments/assets/783af0c4-a84f-4bcb-aa9e-a038671b6615" />


**🎯 Advantages of Multi-Stage Docker Build**

| Feature                      | Benefit                                         |
| ---------------------------- | ----------------------------------------------- |
| Build and runtime separation | Smaller final image                             |
| Git clone inside Docker      | No need to install .NET locally                 |
| Secure                       | Build tools and git not included in final image |
| Reliable                     | Same environment always used for build          |

